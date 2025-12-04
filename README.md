# Soccer-Team
A mobile app that allows groups of friends to create balanced soccer teams based on player stats, schedule matches, track real-time scores, update player stats after matches, and visualize player progress and achievements.
Target Users:

Amateur soccer players

Teams of friends or local players

Anyone wanting fair, balanced matches

2. Core Features
2.1 Player Accounts

Login with Google or Facebook

Profile includes:

Name, profile picture, email, address

Positions: Attack / Midfield / Defense (player can have multiple positions)

Stats per position: Speed, Stamina, Dribbling, Passing, Shooting, Defending

Overall calculated stats

Badges: Top Scorer, Best Defender (per match & all-time)

Friends system: Search, friend requests, contact via Messenger or phone

2.2 Match Creation & Scheduling

Organizer creates match → selects field (GPS verification)

Can schedule match for future date/time

Sends invitations to players → match only confirmed if all invited players accept

Teams can be auto-generated (balanced by total attributes) or manually assigned

Team sizes: 5v5 → 11v11

2.3 Football Field Management

Preloaded football fields (Tunisia)

Players can add new fields → verified via Google Maps → pending confirmation if unverified

Organizer must be near the field to start match timer

2.4 Match Features

Live match timer

Score tracking: Only organizer updates scores/events

Teams displayed with total stats & player info

Stats events are logged per minute (goal, assist, yellow/red card, etc.)

2.5 Post-Match Features

Player Stats Voting:

All participants vote on each other (teammates + opponents)

Votes averaged to update stats

Stats per position updated automatically → progression over multiple matches

Badges awarded for Top Scorer & Best Defender

Match history saved → can be viewed later

2.6 Visualization

Player profile: Radar charts for stats, badge display, match history

Teams: Radar/line charts for total team stats

Leaderboards: All-time and per-match badges

3. Database Design (Supabase)
3.1 Users / Players
Column	Type	Notes
id	UUID	Primary key
name	text	Player full name
email	text	Unique
profile_picture	text	URL
google_facebook_id	text	OAuth login
address	text	Player home address
positions	jsonb	{attack:true, midfield:true, defense:false}
stats_attack	jsonb	{speed:5, stamina:6, dribbling:7, passing:6, shooting:5, defending:3}
stats_midfield	jsonb	Same
stats_defense	jsonb	Same
overall_stats	jsonb	Calculated
badges	jsonb	{top_scorer:5, best_defender:3}
friends_list	array(UUID)	Optional
created_at	timestamp	Account creation
3.2 Football Fields
Column	Type	Notes
id	UUID	Primary key
name	text	Field name
location	geography	Lat/Lng
verified_status	boolean	True/False
created_by	UUID	Player who added it
created_at	timestamp	Timestamp
3.3 Matches
Column	Type	Notes
id	UUID	Primary key
organizer_id	UUID	FK → Users.id
field_id	UUID	FK → Football Fields
datetime	timestamp	Scheduled time
team_size	integer	5–11
status	text	scheduled, live, finished
invited_players	array(UUID)	All invited IDs
accepted_players	array(UUID)	Players who accepted
live_score_team1	integer	Only organizer updates
live_score_team2	integer	Only organizer updates
events_log	jsonb	{minute:12, event:"goal", player_id:xxx}
teams_generated	boolean	Auto-generated teams?
created_at	timestamp	
3.4 Teams
Column	Type	Notes
id	UUID	Primary key
match_id	UUID	FK → Matches.id
team_number	integer	1 or 2
player_ids	array(UUID)	Players in team
total_stats	jsonb	Sum of all players’ stats
3.5 Stats Votes
Column	Type	Notes
id	UUID	Primary key
match_id	UUID	FK → Matches.id
voter_id	UUID	FK → Users.id
target_player_id	UUID	FK → Users.id
stats_update	jsonb	{speed:+1, shooting:+2}
created_at	timestamp	Vote timestamp

Only players who played can vote

Stats averaged → player progression

3.6 Badges
Column	Type	Notes
id	UUID	Primary key
player_id	UUID	FK → Users.id
match_id	UUID	Nullable → for all-time badges
badge_type	text	top_scorer / best_defender
count	integer	1 per match, cumulative all-time
awarded_at	timestamp	
4. App Screens / Wireframe Flow

Login Screen – Google / Facebook

Home / Dashboard – Upcoming matches, quick create match button

Player Directory – Search, friend requests, add friends

Match Creation Screen – Select field (GPS), schedule, invite players, choose team size

Team Assignment Screen – Auto-generate or manual teams

Live Match Screen – Timer, organizer-only score updates, events log, team stats view

Post-Match Stats Voting – Vote on players’ stats

Player Profile Screen – Stats radar charts, badges, match history

Leaderboard Screen – Top scorers, best defenders, overall stats

Field Management Screen – Add or verify football fields

5. Flutter Development Notes

State Management: Riverpod / Provider

Charts: fl_chart for radar/line/bar charts

Authentication: Supabase Auth (Google / Facebook)

Real-time: Supabase subscriptions for match live score and stats updates

Storage: Supabase Storage for profile pictures & field images

Maps: google_maps_flutter for field verification & display

Notifications: Local notifications or push (optional) for match invites

6. App Flow Summary

Login → profile setup

Browse players / search friends

Create match → select field → send invitations

Players accept → match scheduled

Teams generated (auto/manual)

Live match → only organizer updates score → all see real-time score/events

Post-match stats voting → update player stats → progression & badges

Match history → player profile → leaderboards

7. Development Roadmap (Step-by-Step)
Phase 1: Backend

Set up Supabase project

Create tables: Users, Matches, Teams, Stats Votes, Badges, Fields

Set up authentication (Google/Facebook)

Set up storage for images

Implement real-time subscriptions for live matches

Phase 2: Flutter Skeleton

Login & home screen

Player directory screen

Match creation & team assignment

Live match screen with organizer-only score updates

Post-match voting screen

Phase 3: Visualization & Charts

Player profile radar chart

Team stats comparison charts

Badges display

Phase 4: Scheduling & Notifications

Scheduled matches list

Push notifications for invitations & match start

Phase 5: Testing & Deployment

Test real-time updates, score submission, and stats voting

Test GPS verification

Deploy APK for Android (later iOS)

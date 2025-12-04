# Soccer Team Balancer ⚽

[![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev/)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

## 📌 Project Overview
**Platform:** Mobile (Android & iOS) using **Flutter**  
**Backend:** **Supabase** (PostgreSQL + Real-time + Authentication + Storage)  

A mobile app that allows friends and local players to create **balanced soccer teams** based on player stats, schedule matches, track **real-time scores**, update player stats after matches, and visualize player **progress and achievements**.

**Target Users:**  
- Amateur soccer players  
- Teams of friends or local players  
- Anyone wanting fair, balanced matches  

---

## 🚀 Core Features

### Player Accounts
- Login with **Google** or **Facebook**  
- Profile includes name, profile picture, email, address  
- Positions: Attack / Midfield / Defense (multiple)  
- Stats per position: Speed, Stamina, Dribbling, Passing, Shooting, Defending  
- Overall calculated stats  
- Badges: Top Scorer, Best Defender (per match & all-time)  
- Friends system: search, friend requests, contact via Messenger/phone  

### Match Creation & Scheduling
- Organizer creates match → selects field (**GPS verification**)  
- Schedule matches for future date/time  
- Invitations to players → match confirmed when all accept  
- Teams can be **auto-generated** (balanced) or **manual**  
- Team sizes: **5v5 → 11v11**

### Football Field Management
- Preloaded fields (Tunisia)  
- Players can add new fields → verified via Google Maps  
- Organizer must be near field to start match timer  

### Match Features
- Live match timer  
- Score tracking (only organizer updates scores/events)  
- Teams displayed with total stats & player info  
- Events logged per minute (goal, assist, yellow/red card)  

### Post-Match Features
- **Player Stats Voting:** participants vote on each other  
- Stats per position updated automatically → progression  
- Badges awarded for Top Scorer & Best Defender  
- Match history saved → view later  

### Visualization
- Player profile: radar charts for stats, badges, match history  
- Teams: radar/line charts for total stats  
- Leaderboards: all-time & per-match badges  

---

## 🗄 Database Design (Supabase)

### Users / Players
| Column | Type | Notes |
|--------|------|------|
| id | UUID | Primary key |
| name | text | Player full name |
| email | text | Unique |
| profile_picture | text | URL |
| google_facebook_id | text | OAuth login |
| address | text | Player home address |
| positions | jsonb | {attack:true, midfield:true, defense:false} |
| stats_attack | jsonb | {speed:5, stamina:6, ...} |
| stats_midfield | jsonb | Same |
| stats_defense | jsonb | Same |
| overall_stats | jsonb | Calculated |
| badges | jsonb | {top_scorer:5, best_defender:3} |
| friends_list | array(UUID) | Optional |
| created_at | timestamp | Account creation |

> **Other tables:** Football Fields, Matches, Teams, Stats Votes, Badges (see full project doc for structure)  

---

## 📱 App Screens / Flow
1. Login Screen – Google / Facebook  
2. Home / Dashboard – upcoming matches, quick create button  
3. Player Directory – search, friend requests, add friends  
4. Match Creation – select field, schedule, invite players, team size  
5. Team Assignment – auto or manual teams  
6. Live Match – timer, organizer-only score updates, event log  
7. Post-Match Stats Voting – vote on player stats  
8. Player Profile – radar charts, badges, match history  
9. Leaderboard – top scorers & defenders  
10. Field Management – add/verify fields  

---

## 🛠 Flutter Development Notes
- **State Management:** Riverpod / Provider  
- **Charts:** fl_chart (radar/line/bar)  
- **Auth:** Supabase Auth (Google/Facebook)  
- **Real-time:** Supabase subscriptions  
- **Storage:** Supabase Storage for profile & field images  
- **Maps:** google_maps_flutter for field verification  
- **Notifications:** local or push for match invites  

---

## ⚙ App Flow Summary

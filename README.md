# Darbak | دربك

<p align="center">
  <img src="docs/images/darbak-logo.png" alt="Darbak logo — دربك" width="280">
</p>

<p align="center">
  A ride-sharing and match-planning platform for football fans attending the <b>AFC Asian Cup in Saudi Arabia</b>.<br>
  Fans can offer seats, request rides, save matches, and use AI to plan their matchday.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-17-orange" alt="Java 17">
  <img src="https://img.shields.io/badge/Spring%20Boot-REST%20API-6DB33F" alt="Spring Boot">
  <img src="https://img.shields.io/badge/MySQL-Database-4479A1" alt="MySQL">
  <img src="https://img.shields.io/badge/Maven-Build-C71A36" alt="Maven">
</p>

---

## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Technology](#technology)
- [My contribution](#my-contribution)
- [API endpoints](#api-endpoints)

---

## Overview

Darbak (دربك — "your way") connects football fans travelling to the same match. Drivers publish rides with available seats, passengers search and request seats, and the platform handles approvals, notifications, reviews, and AI-powered matchday planning.

This is a team capstone project built during the **Java & AI Web Development program at Tuwaiq Academy**.

## Features

- **Rides:** ride search, car availability checks, and seat reservations.
- **Bookings:** drivers accept or reject requests and manage passengers.
- **Matches:** saved matches, ride recommendations, and saved matches without arranged rides.
- **Maps:** Google Maps meeting points and directions links.
- **AI:** matchday plans, two-match attendance estimates, review moderation, and review summaries.
- **Users:** ratings, statistics, automatic WhatsApp notifications, and welcome emails.
- **Admin:** match/stadium management, Football API fixture import, and user banning.

## Technology

| Area | Stack |
|---|---|
| Backend | Java 17 · Spring Boot · Spring Data JPA / Hibernate · Jakarta Validation · Lombok · Maven |
| Database | MySQL |
| Integrations | OpenRouter AI · Google Maps JavaScript API · API-Football · UltraMsg WhatsApp · Spring Mail (Gmail SMTP) · OpenPDF |
| Demo frontend | HTML · CSS · JavaScript |

---

## My contribution

I (**Dana Alshammary**) was responsible for the **user, analytics, and AI planning** endpoints — **11 endpoints** in total:

| Method | Path | Purpose |
|---|---|---|
| POST | `/user/register` | Register a user and send a welcome email |
| POST | `/user/login/{email}/{password}` | Login |
| GET | `/user/search/{name}` | Search users by name |
| GET | `/user/above-average-rating` | Users above the average rating |
| GET | `/user/below-average-rating` | Users below the average rating |
| GET | `/user/average-rating/{userId}` | A user's average rating |
| GET | `/user/ride-statistics/{userId}` | A user's ride statistics |
| GET | `/user/match-statistics/{userId}` | Statistics for matches connected through rides |
| GET | `/user/upcoming-rides/{userId}` | A user's upcoming rides |
| GET | `/user/ai-matchday-plan/{userId}/{lang}` | AI-generated matchday plan in `en` or `ar` |
| POST | `/user/send-plan/{userId}` | Generate the user's plan as a PDF and send it by email |

**Highlights**

- Welcome email on registration using Spring Mail (Gmail SMTP).
- Rating analytics comparing each user against the platform-wide average.
- Ride and match statistics aggregated per user.
- Bilingual (English / Arabic) AI matchday plan generated through OpenRouter.
- PDF plan generation with OpenPDF, delivered by email.

---

## API endpoints

All routes use the prefix **`/api/v1`**.
The project has **46 feature endpoints** (listed below) plus **34 basic CRUD endpoints**, which are listed in [Endpoint allocation](ENDPOINT_ALLOCATION.md).

### Users and analytics — 11

| Method | Path | Purpose |
|---|---|---|
| POST | `/user/register` | Register and send welcome email |
| POST | `/user/login/{email}/{password}` | Login |
| GET | `/user/search/{name}` | Search users by name |
| GET | `/user/above-average-rating` | Users above average rating |
| GET | `/user/below-average-rating` | Users below average rating |
| GET | `/user/average-rating/{userId}` | User's average rating |
| GET | `/user/ride-statistics/{userId}` | User's ride statistics |
| GET | `/user/match-statistics/{userId}` | Statistics for matches connected through rides |
| GET | `/user/upcoming-rides/{userId}` | User's upcoming rides |
| GET | `/user/review-summary/{userId}` | AI review summary |
| GET | `/user/ai-matchday-plan/{userId}/{lang}` | AI matchday plan in `en` or `ar` |

### Saved matches and email plans — 5

| Method | Path | Purpose |
|---|---|---|
| GET | `/user/{userId}/matches` | Get saved matches |
| POST | `/user/{userId}/matches/{matchId}` | Save a match |
| DELETE | `/user/{userId}/matches/{matchId}` | Remove a saved match |
| GET | `/user/{userId}/matches/without-rides` | Upcoming saved matches without arranged rides |
| POST | `/user/send-plan/{userId}` | Email the user's plan as a PDF |

### Rides, maps, and vehicle availability — 10

| Method | Path | Purpose |
|---|---|---|
| POST | `/Ride/add/{driverId}/{matchId}/{carId}` | Create a ride with ownership, capacity, and conflict checks |
| GET | `/Ride/search/{matchId}/{date}/{seats}` | Find suitable rides |
| GET | `/Ride/available` | List available rides |
| GET | `/Ride/match/{matchId}` | Get rides for a match |
| GET | `/Ride/driver/{driverId}` | Get a driver's rides |
| PUT | `/Ride/complete/{rideId}/{driverId}` | Complete a departed ride |
| PUT | `/Ride/cancel/{rideId}/{driverId}` | Cancel a ride before departure |
| GET | `/Ride/map/{rideId}` | Coordinates and Google Maps directions links |
| GET | `/Ride/recommendations/user/{userId}` | Recommend available rides for saved matches |
| GET | `/car/availability-conflict/{carId}` | Check whether a car has an overlapping ride |

### Booking requests and participants — 11

| Method | Path | Purpose |
|---|---|---|
| POST | `/RideRequest/add/{user_id}/{ride_id}` | Request a seat |
| GET | `/RideRequest/ride/{ride_id}` | Get requests for a ride |
| GET | `/RideRequest/user/{user_id}` | Get a user's requests |
| POST | `/RideRequest/accept/{request_id}/{driver_id}` | Accept a request and reserve a seat |
| POST | `/RideRequest/reject/{request_id}/{driver_id}` | Reject a request |
| DELETE | `/RideRequest/cancel/{request_id}/{passenger_id}` | Cancel a pending request |
| GET | `/RideRequest/user/{user_id}/pending` | Get pending requests |
| GET | `/RidePerticipant/user/{user_id}/ride/{ride_id}` | Get a specific participant |
| GET | `/RidePerticipant/ride/{ride_id}` | Get a ride's participants |
| GET | `/RidePerticipant/ride/{ride_id}/role/{role}` | Filter participants by `driver` or `passenger` |
| GET | `/RidePerticipant/passenger-count/{rideId}` | Count passengers |

### Reviews, matches, and administration — 9

| Method | Path | Purpose |
|---|---|---|
| POST | `/review/add` | Review a completed ride (AI comment moderation) |
| PUT | `/review/update/{id}` | Update a review (AI comment moderation) |
| GET | `/match/stadium/{stadiumId}` | Matches at a stadium |
| GET | `/match/team/{teamName}` | Matches involving a team |
| GET | `/match/city/{city}` | Matches in a city |
| GET | `/match/attendance-check/{match1_id}/{match2_id}` | AI two-match attendance estimate |
| POST | `/stadium/add-all` | Add multiple stadiums, skipping existing names |
| POST | `/football/import-test-matches` | Import historical Asian Cup fixtures with shifted test dates |
| PUT | `/admin/ban/{userId}` | Ban a user from logging in |

---


# WebBugX — Debug. Detect. Dominate.

College symposium event management system for **26 September 2026**, **3rd Floor, AIDS Lab 1**.

## Event flow

1. Participant registers with:
   - Full Name
   - College Name
   - Email
   - Phone Number
2. Participant logs in with the same four details. No participant password is used.
3. Everyone automatically proceeds through all three rounds.
4. Final ranking:
   - Higher total score first.
   - If scores are tied, lower total completion time first.
5. Admin verifies Round 2 and Round 3 where required and can see the final leaderboard.

## Rounds

- **Round 1 — Bug Hunt Quiz:** 15 MCQs × 1 = 15 marks, 10 minutes.
- **Round 2 — Code Debugging:** 5 questions × 5 = 25 marks, 20 minutes.
- **Round 3 — Output to Reality:** 2 questions × 10 = 20 marks, 20 minutes.
- **Total:** 60 marks.

## Technology

- Java 17
- Spring Boot 4.0.4
- Spring Data JPA
- MySQL
- HTML / CSS / JavaScript
- No React

## Open in Eclipse

1. Extract the ZIP.
2. Open Eclipse.
3. Choose **File → Import → Maven → Existing Maven Projects**.
4. Select the extracted **WebBugX** project folder containing `pom.xml`.
5. Click **Finish**.
6. Wait for Maven dependencies to finish downloading.
7. Make sure MySQL is running.
8. Create the database if it does not exist:

```sql
CREATE DATABASE webbugx;
```

9. Check `src/main/resources/application.properties` and change the MySQL username/password if needed.
10. Open `WebBugXApplication.java`.
11. Right-click → **Run As → Spring Boot App**.
12. Open:

`http://localhost:8080/index.html`

**Do not double-click the HTML files.** Do not use `file:///.../index.html`. The pages must be served by Spring Boot because they call the `/api` backend.

## Admin

Open:

`http://localhost:8080/admin.html`

Default local-event admin account:

- Email: `admin@webbugx.local`
- Password: `WebBugX@2026`

Change these values in `application.properties` before the actual event if required.

## College Wi-Fi deployment

Run Spring Boot on the organizer/admin computer.

Find the server computer's LAN IPv4 address, for example:

`192.168.1.20`

Allow TCP port **8080** through the Windows firewall.

Participant computers on the same college Wi-Fi can then open:

`http://SERVER-IP:8080/index.html`

Example:

`http://192.168.1.20:8080/index.html`

The event website itself does not require public internet access once the Spring Boot server, MySQL and participant devices are on the college LAN.

## Important implementation notes

- Participant question APIs never send `correctAnswer`.
- Admin question APIs can show the hidden correct/reference answers.
- Round 2 is stored for manual admin scoring.
- Round 3 has automatic normalized code checking plus admin verification.
- Round 3 submitted code is stored in question order.
- Round timers are based on the server-side attempt start time, so refreshing a page does not reset the timer.
- The backend also checks the round time limit when a submission arrives.
- Participants cannot skip ahead to another round.
- The final leaderboard contains participants who completed all three rounds.
- Winner and runner-up are determined from score first and total completion time second.

## Question data

The supplied Round 2 buggy/corrected files and Round 3 buggy/corrected files are in:

`src/main/resources/data/`

Round 3 target images are in:

`src/main/resources/static/targets/`

The application refreshes the question table from these source files when it starts, while participant and round-attempt data remain untouched.

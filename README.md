# ResQ — Emergency Response Command Center

ResQ is a web-based emergency incident reporting and coordination dashboard built for the Code Clash 2026 project. It brings incident reports into one workspace, helps coordinators review priority and possible duplicate reports, and includes a safe disaster-response simulation for demonstrations.

> **Important:** ResQ is a prototype and is not connected to official emergency dispatch services. It does not contact responders or send help. For an immediate emergency in India, call **112** directly. Do not wait for this website to request assistance.

## Live project

- **Application:** https://code-clash-2026.hatchable.site/
- **Hosting / project platform:** Hatchable
- **Project version in the latest supplied screenshot:** v26

## Features

### 1. Emergency incident dashboard

- Overview of submitted incidents and severity counts.
- Incident feed with search and severity filtering.
- Status updates for reports.
- Category, severity and response-status charts.
- Incident map and location pinning where coordinates are available.
- Emergency contact directory.

### 2. Multilingual voice emergency reporting

- Browser-based speech recognition where supported.
- Language choices include English (India), Telugu, Hindi, Tamil, Kannada, Malayalam, Bengali, Marathi and Urdu.
- Editable transcript so a reporter can review the text before submitting.
- Manual text entry is available when speech recognition or a language pack is unavailable.
- Short incident summary and suggested severity are generated in the browser from the entered report text.
- Optional device GPS location can be attached with the reporter's permission.

Speech-recognition availability varies by browser, device, network and installed language support. The language menu does not guarantee that every browser can transcribe every language. Always review the transcript before submitting it.

### 3. Duplicate incident detection

The shared reporting endpoint compares recent reports using:

- Incident category.
- Time of submission.
- Text similarity in the description.
- Written location and, when available, GPS distance.

Potential matches are grouped under a common incident group. Original reports are retained rather than discarded, and the interface displays the number of reports and distinct browser sources in the group.

The source count is based on a pseudonymous browser key stored in browser local storage. It is **not identity verification** and should not be described as a verified count of individual people. Similarity matches are suggestions for human review, not proof that two reports describe the same event.

### 4. Disaster simulation mode

Predefined demonstration scenarios:

- Urban flooding.
- Earthquake response.
- Industrial fire.
- Cyclone and severe weather.
- Multiple incident surge.

The simulator generates synthetic incidents, sorts the queue by severity, suggests a demonstration team based on category, changes sample team and incident statuses, and displays progress metrics and an event timeline. Start, pause, resume and reset controls are provided.

**Simulation data is kept in page memory only.** It is not inserted into the shared incident database and does not dispatch real teams. Team allocation and response status changes are illustrative demo behaviour only.

## Technology

- HTML for the application interface.
- CSS for layout, responsive styling and the dark ResQ theme.
- Vanilla JavaScript for dashboard interactions and simulation behaviour.
- Browser Web Speech API (`SpeechRecognition` / `webkitSpeechRecognition`) for voice input where supported.
- Browser Geolocation API for optional GPS capture.
- Leaflet and OpenStreetMap tiles for map visualisation.
- Chart.js for dashboard charts.
- Hatchable for hosting, serverless API routes and managed PostgreSQL.
- PostgreSQL for shared incident submissions and duplicate-group counts.
- Hatchable AI integration for the existing ResQ Assistant (requires an AI provider connection).

## Repository structure

The following structure reflects the ResQ project files in Hatchable at the time this README was prepared. Include the actual source files in the GitHub repository as well as this README.

```text
resq/
├── README.md
├── hatchable.toml
├── api/
│   ├── assistant.js
│   └── intelligence.js
├── migrations/
│   └── 0001_create_resq_incidents.sql
└── public/
    ├── app.js
    ├── cursor-fix.css
    ├── index.html
    ├── intelligence.css
    ├── intelligence.js
    ├── login.html
    ├── styles.css
    └── theme.css
```

## Running and deploying

This project currently uses Hatchable-specific serverless routes and the Hatchable SDK. It is **not a standalone Node.js project** that can be run by simply executing `npm install` or `npm start`.

To continue working on it in Hatchable:

1. Open the ResQ project in the Hatchable console.
2. Use the Code or file editor to access the project files listed above.
3. Keep the `api/`, `migrations/`, `public/` folders and `hatchable.toml` in the expected paths.
4. Deploy through Hatchable so the migration is applied and the API route becomes available.

To move the project to another hosting provider, export the source files and database data, then replace Hatchable SDK calls and platform authentication with equivalents supported by the new host. The source code alone does not recreate Hatchable's managed database, authentication, AI gateway or deployment environment.

## Configuration notes

### AI Assistant

The Hatchable project configuration declares an AI capability. The existing `/api/assistant` route uses a Gemini model. The Hatchable console screenshot supplied with this project showed **“Missing: AI”**. Connect a supported Google/Gemini provider key in the Hatchable AI settings if the AI Assistant is required. Voice transcription and the local incident-summary helper do not depend on this AI connection.

### Public visibility

The supplied Hatchable console screenshot shows the Visibility setting as **Open to the web**. Recheck the project visibility in Hatchable before sharing the live application. Never put API keys, private credentials or personal incident data in this repository.

### Database

The migration `migrations/0001_create_resq_incidents.sql` creates the `resq_incidents` table and indexes used by the shared incident API. The application also retains browser-local dashboard data for its original demo records and preferences. Browser-local records are not automatically shared with other devices.

## Limitations and responsible use

- ResQ is a student prototype, not a certified emergency response system.
- The dashboard does not connect to government control rooms or dispatch services.
- Incident severity suggestions are rule-based hints, not professional risk assessments.
- Duplicate detection can produce false positives and false negatives; a coordinator must review suspected groups.
- Distinct browser-source counts do not prove the number or identity of people reporting.
- Browser speech recognition and location access depend on device/browser support and user permissions.
- Do not use synthetic simulation records as real incident data.
- Do not expose real people's contact details, exact sensitive locations or other personal information in a public GitHub repository.

## Project status

This README documents the ResQ implementation in Hatchable as of the supplied project screenshot and the v26 deployment details. Update this document whenever the source files, feature behaviour or deployment configuration changes.

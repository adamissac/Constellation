# Constellation

### Less networking. More connection.

Meet the people you have something in common with—and turn a conversation into a connection worth keeping. Constellation combines AI-powered matching, an interactive constellation of shared interests, and mutual consent to make networking more personal.

Built for **HackGT 13** · **AI/ML + Data Visualization**

[![CI](https://github.com/adamissac/Constellation/actions/workflows/ci.yml/badge.svg)](https://github.com/adamissac/Constellation/actions/workflows/ci.yml)

[Dashboard](https://formal-connection-dashboard.vercel.app) · [Watch the trailer](trailer/renders/constellation-trailer-16x9.mp4) · [Run the app](#run-the-app) · [Documentation](#documentation)

[![Constellation trailer poster](trailer/renders/constellation-poster-16x9.png)](trailer/renders/constellation-trailer-16x9.mp4)

## Why Constellation?

Meeting someone interesting should take more than collecting a profile. Constellation helps you find common ground, start a conversation, and stay in touch afterward. Your connection list is private, and connecting requires both people to choose it.

## What you can do

- **Build an interest profile.** Connect GitHub, upload a resume, or describe your interests. Review the evidence behind extracted skills, confirm what fits, and hide what does not.
- **Find relevant people.** Get ranked matches with shared topics, overlap breakdowns, and explanations of why you might want to talk.
- **Explore your constellation.** An animated star graph turns matching into something you can explore. Color shows shared-interest categories; larger match stars indicate higher scores.
- **Meet with mutual consent.** Open to Meet, private invitations, conversation starters, and chat help introductions move beyond a profile.
- **Verify a real connection.** In-person verification and a private post-conversation checklist lead to a connection only when both people say yes.
- **Stay in the loop.** See posts and GitHub activity from your connections, with AI summaries and suggested replies. Your main feed excludes your own activity.
- **Host and attend events.** Companies create events and share check-in QR codes. Attendees register and check in; organizers see aggregate registration and attendance counts.

Active screens refresh automatically. Most cross-device updates use short foreground polling intervals; chat also uses Supabase Realtime. Bluetooth features require a native development build.

## A look inside

<p>
  <img src="trailer/public/screens/graph.png" width="240" alt="Constellation matching graph with stars representing people" />
  <img src="trailer/public/screens/match_maya.png" width="240" alt="Match profile showing shared interests and a conversation starter" />
  <img src="trailer/public/screens/feed.png" width="240" alt="Connections feed with activity from other people" />
</p>

Screenshots use synthetic demo people. This is a hackathon prototype; available behavior depends on the client build and configured services.

## How it works

1. **Share your interests:** add your GitHub, resume, and what you are looking for or can offer.
2. **Find common ground:** review matches and explore shared interests in the constellation.
3. **Choose to meet:** mutual interest unlocks the next step without revealing a one-sided response.
4. **Have a conversation:** verify the meeting and privately decide whether to connect.
5. **Keep the connection:** follow each other's work through a private feed and chat.

## Privacy comes first

- Your connections and connection count are visible only to you.
- Declining or ignoring a suggestion does not notify the other person.
- Discovery is limited to eligible matches and your connections; there is no public stranger search.
- Temporary meetup location sharing requires mutual consent. Nearby browsing pins do not represent another person's precise position.
- LinkedIn is used for identity, not scraping. GitHub data comes through its official API.
- Bluetooth identifiers rotate, and proximity information is used for verification under the project's retention rules.

The complete privacy requirements are in [MASTER_SPEC.md](MASTER_SPEC.md), Sections 1.3 and 11.

## Architecture

| Component | Technology | Directory |
| --- | --- | --- |
| Mobile app | React Native, Expo, Expo Router, TypeScript | [`mobile/`](mobile/) |
| Matching and API | Python, FastAPI, sentence-transformers, scikit-learn, LightGBM | [`ml/`](ml/) |
| Data and identity | Supabase Auth, Postgres, pgvector, Storage, Realtime | [`supabase/`](supabase/) |
| Dashboard | Next.js, React, interactive graph visualization | [`dashboard/`](dashboard/) |
| Product trailer | Remotion | [`trailer/`](trailer/) |

The matching system combines interest similarity, rare shared topics, complementary goals, and cross-disciplinary overlap. Generated explanations and conversation starters are grounded in the information people choose to share.

## Run the app

You will need Git, Node.js/npm, and Expo Go for the mobile preview. The phone-QR helper also uses Python 3. Bluetooth scanning and advertising require a development build rather than Expo Go.

```bash
git clone https://github.com/adamissac/Constellation.git
cd Constellation/mobile
npm install
cd ..
./scripts/phone-qr.sh
```

Scan the generated QR with Expo Go and keep the terminal open. The helper starts an Expo tunnel and saves a QR image to your Desktop, or the repository if no Desktop directory exists. A tunnel is needed for the project's Expo Go authentication redirects.

The helper creates `mobile/.env` from its example if missing. Keep existing values when updating. To explore sample data, use **Try the demo** on the sign-in screen; demo activity is local and does not synchronize between phones.

For real Bluetooth functionality, follow the [iOS development-build guide](docs/ios-dev-build.md). For the backend, see [deployment setup](docs/deploy.md) and [environment setup](docs/secrets-setup.md). Do not commit credentials or put service-role keys in client code.

### Dashboard

```bash
cd dashboard
npm install
npm run dev
```

The public [dashboard deployment](https://formal-connection-dashboard.vercel.app) is separate from the Expo mobile app.

### Checks

```bash
# From mobile/
npm run typecheck
npm run lint
npm run test:demo

# From dashboard/
npm run typecheck
npm run lint
npm test
npm run build
```

Backend setup requires Python 3.10–3.13; see [AGENTS.md](AGENTS.md) for environment and test commands. Database integration tests require a dedicated throwaway Postgres database with pgvector—never use the production database.

## Documentation

- [Master specification](MASTER_SPEC.md) — product, privacy, architecture, and task ownership.
- [API contracts](docs/api.md) and [database schema](docs/schema.sql).
- [Example API payloads](docs/mocks/).
- [Development rules](AGENTS.md) and [Git guide](docs/git-cheatsheet.md).
- [Latest progress and handoffs](PROGRESS.md) and [team requests](REQUESTS.md).
- [Backend deployment](docs/deploy.md) and [iOS device setup](docs/ios-dev-build.md).
- [Trailer source and rendering instructions](trailer/README.md).

Local previews serve the files on the computer running them. Hot reload alone does not fetch GitHub changes. The phone-QR helper checks for updates while it runs; follow the preview synchronization rules in [AGENTS.md](AGENTS.md) and preserve uncommitted work.

## Team

| Team member | Focus |
| --- | --- |
| Adam | Full-stack, mobile experience, authentication, and Supabase |
| Alan | AI/ML, matching, ranking, and API services |
| Arjun | Research, data ingestion, and visualization |
| Akshar | Bluetooth, proximity verification, and integrations |

Found an issue? [Open a GitHub issue](https://github.com/adamissac/Constellation/issues) with what happened, the screen involved, and whether you were using Expo Go, a development build, or the web dashboard. Keep private account data and credentials out of reports.

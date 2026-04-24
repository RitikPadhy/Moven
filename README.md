# Moven

> You already wear the sensors. We just make them talk to the right people.

Moven connects the motion and health data from your existing watch and phone to the people who need it most — your trainer, physio, or coach. No extra hardware. No recording videos. No manual steps. Just wear your watch, carry your phone, and let Moven handle the rest.

Built for remote personal training, post-surgical rehab, and anyone who shouldn't have to drive 5km just to hear *"your form looks good today."*

---

## The Problem

Right now, the feedback loop between a patient or athlete and their trainer looks like this:

```
You work out alone
        ↓
Nobody knows if your effort was right
Nobody knows if your form broke down
Nobody knows if you're compensating
        ↓
You wait until the next in-person session
        ↓
By then, the damage is already done
```

For ACL rehab patients, remote athletes, and anyone working with a trainer they can't see daily — this gap is real, frustrating, and in some cases dangerous.

---

## The Solution

Moven passively collects motion and health data from the sensors you already carry, detects anomalies using AI, and closes the feedback loop with your trainer or physio — automatically.

```
You work out
        ↓
Watch tracks effort, movement, and recovery
Phone passively monitors gait and compensation
        ↓
Moven AI detects anomalies
        ↓
Trainer gets a clear, plain-English session report
        ↓
Trainer responds with feedback
        ↓
You get a notification
```

No setup. No recording. No extra hardware.

---

## How It Works

### For the User
- Strap on your watch. Carry your phone. Work out.
- After your session, Moven generates a movement report automatically.
- Your trainer reviews it and sends feedback.
- You receive a notification with their response before your next session.

### For the Trainer or Physio
- Get notified when a client completes a session.
- Review a clean AI-generated report — effort score, anomalies flagged, trend data.
- Respond with a voice note or text in under 2 minutes.
- Monitor all clients from a single dashboard without watching a single video.

---

## What Moven Detects

### From Apple Watch / Wearable (Effort Layer)
| Signal | What It Tells Us |
|---|---|
| Heart rate zones | Was intensity appropriate for the session goal? |
| HRV post-workout | Did the body recover well? |
| Active calories | Was energy output in the right range? |
| Movement velocity | Were reps controlled or rushed? |
| Inter-rep variability | Did form degrade as fatigue set in? |
| Range of motion trend | Is the client guarding or compensating over time? |

### From iPhone Passive Sensors (Compensation Layer)
| Signal | What It Tells Us |
|---|---|
| Walking asymmetry | Is there a left/right imbalance in daily movement? |
| Walking steadiness | Is stability improving or declining week over week? |
| Step length | Are stride mechanics normalizing post-rehab? |
| Double support time | Is the client gaining confidence on the injured side? |
| Stair climb data | Is functional leg strength improving? |

### Combined AI Analysis
Moven correlates effort data with daily movement patterns to surface insights no single sensor can provide alone.

> *"Alex's walking asymmetry increased 17% this week despite completing all sessions. His descent velocity on lunges has also increased — possible pain avoidance behavior. Recommend physio check-in before next session."*

---

## Anomaly Detection — How It Works

Moven uses a three-layer detection system that improves over time:

**Layer 1 — Rule-Based (Ships First)**
Clinical rehab thresholds encoded directly from physiotherapy best practices. Flags known red flags like loss of eccentric control, asymmetric loading, and range of motion reduction.

**Layer 2 — Personal Baseline (Adds With Time)**
Builds a statistical model of each individual's normal movement patterns. Flags deviations that are significant for *that person*, not just against a population average.

**Layer 3 — Machine Learning (Grows With Data)**
As trainers confirm or dismiss flags, Moven learns which sensor patterns correlate with real form breakdowns. Every confirmation makes the model smarter across all users.

---

## Use Cases

- **ACL Rehab** — Monitor compensation patterns daily, not just at weekly clinic visits
- **Remote Personal Training** — Give trainers full visibility into client effort and consistency
- **Sports Academies** — Track athlete recovery and flag overtraining before injury
- **Corporate Wellness** — Monitor team fitness engagement and flag early burnout signals
- **Post-Surgical Recovery** — Any lower-body procedure where gait asymmetry is a key recovery metric

---

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   User Device                   │
│                                                 │
│  Apple Watch / Garmin          iPhone           │
│  - Heart rate                  - Accelerometer  │
│  - HRV                         - Gyroscope      │
│  - Motion (6-axis IMU)         - Apple Health   │
│  - Active calories             - Walking metrics│
└──────────────┬──────────────────────┬───────────┘
               │                      │
               ▼                      ▼
┌─────────────────────────────────────────────────┐
│                  Moven Core                     │
│                                                 │
│  Data Ingestion Layer                           │
│  HealthKit API / Google Fit / Garmin Connect    │
│                                                 │
│  Feature Extraction                             │
│  Velocity · Asymmetry · Variability · RoM       │
│                                                 │
│  Anomaly Detection Engine                       │
│  Rules → Statistical → ML                      │
│                                                 │
│  Report Generation (LLM)                       │
│  Plain-English summaries for trainers           │
└──────────────────────────┬──────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────┐
│               Trainer Dashboard                 │
│                                                 │
│  Session reports · Flagged anomalies            │
│  Client trend graphs · Feedback composer        │
│  Voice note · Push notification delivery        │
└─────────────────────────────────────────────────┘
```

---

## Repo Structure

```
moven/
├── core/                   # Anomaly detection engine
│   ├── features/           # Signal extraction from raw IMU data
│   ├── baseline/           # Personal baseline modeling
│   └── rules/              # Clinical rule definitions
├── integrations/           # Watch and health platform connectors
│   ├── apple-health/       # HealthKit integration
│   ├── google-fit/         # Google Fit REST API
│   ├── garmin/             # Garmin Connect API
│   └── whoop/              # WHOOP API
├── api/                    # Backend API
│   ├── sessions/           # Session ingestion and storage
│   ├── reports/            # AI report generation
│   └── notifications/      # Push notification delivery
├── dashboard/              # Trainer web dashboard
├── mobile/                 # iOS / Android client
└── docs/                   # Documentation
```

---

## Getting Started

### Prerequisites
- Node.js 18+
- Python 3.10+
- Apple Developer account (for HealthKit access)
- PostgreSQL 14+

### Installation

```bash
git clone https://github.com/your-org/moven.git
cd moven
npm install
cp .env.example .env
```

### Environment Variables

```env
# Database
DATABASE_URL=postgresql://localhost:5432/moven

# Apple HealthKit
HEALTHKIT_BUNDLE_ID=your.bundle.id

# Garmin Connect API
GARMIN_CLIENT_ID=your_client_id
GARMIN_CLIENT_SECRET=your_client_secret

# WHOOP API
WHOOP_CLIENT_ID=your_client_id
WHOOP_CLIENT_SECRET=your_client_secret

# LLM (for report generation)
ANTHROPIC_API_KEY=your_api_key

# Push Notifications
APNS_KEY_ID=your_key_id
APNS_TEAM_ID=your_team_id
```

### Run Locally

```bash
# Start the API
npm run dev:api

# Start the dashboard
npm run dev:dashboard

# Run the anomaly detection engine
python core/engine.py
```

---

## Contributing

Moven is open source and we welcome contributions. Whether you're fixing a bug, improving documentation, adding a new wearable integration, or improving the anomaly detection engine — you're welcome here.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

### Good First Issues
Look for issues tagged `good first issue` — these are well-scoped tasks that don't require deep domain knowledge to get started.

### Areas We Need Help
- Additional wearable integrations (Polar, Suunto, Samsung Health)
- Android / Wear OS support
- Anomaly detection model improvements
- Clinical validation studies
- Accessibility improvements in the trainer dashboard

---

## Roadmap

**Phase 1 — Foundation** *(Current)*
- [ ] Apple HealthKit integration
- [ ] Basic effort tracking and session reports
- [ ] Trainer dashboard (web)
- [ ] Push notification feedback loop
- [ ] Rule-based anomaly detection

**Phase 2 — Intelligence**
- [ ] Personal baseline modeling
- [ ] Walking asymmetry integration (iPhone passive)
- [ ] Garmin and WHOOP integrations
- [ ] AI-generated plain-English session summaries
- [ ] On-demand video request from trainer

**Phase 3 — Scale**
- [ ] Machine learning anomaly detection
- [ ] Multi-client trainer dashboard
- [ ] Android / Google Fit support
- [ ] API for third-party integration
- [ ] HIPAA compliance infrastructure

---

## Why Open Source

The fitness and rehab technology space is fragmented, expensive, and largely inaccessible to individual trainers and patients who need it most. By building Moven in the open, we want to:

- Let trainers and physios audit exactly what data is being collected
- Enable researchers to build on top of the movement analysis infrastructure
- Allow the community to add integrations we haven't thought of yet
- Keep the core tool free for individuals while building sustainable enterprise features

---

## License

Moven core is licensed under the [MIT License](LICENSE).

---

## Acknowledgements

Built for every patient who drove further than they should have just to be told their form looks fine.

---

*Moven — move better, recover smarter.*

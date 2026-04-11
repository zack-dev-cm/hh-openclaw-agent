# HH OpenClaw Agent

**Prepare a reviewed `hh.ru` application packet, execute the browser flow through OpenClaw, and capture one auditable submission bundle.**

HH OpenClaw Agent is a small public OpenClaw skill for `hh.ru` application work. It creates
an application packet, records OpenClaw execution steps, checks the resulting submission bundle, and renders
a shareable markdown report for review, debugging, or funnel tracking.

## Quick Start

```bash
python3 skill/hh-openclaw-agent/scripts/init_hh_application_packet.py \
  --out /tmp/hh-packet.json \
  --packet-id hh-demo-001 \
  --vacancy-id 135791357 \
  --vacancy-title "Senior ML Engineer" \
  --company-name "Example AI" \
  --resume-title "Senior ML Engineer / AI Systems" \
  --vacancy-url 'https://hh.ru/vacancy/135791357' \
  --apply-url 'https://hh.ru/applicant/vacancy_response?vacancyId=135791357' \
  --browser-profile hh-main \
  --cover-letter "Здравствуйте. Мне интересна эта роль, потому что у меня сильный практический опыт в production ML и AI delivery."

python3 skill/hh-openclaw-agent/scripts/append_hh_execution_step.py \
  --manifest /tmp/hh-packet.json \
  --step-id open-response-flow \
  --action "Open the direct vacancy response page in a logged-in profile" \
  --expected "The response form loads and the cover letter field is visible" \
  --actual "The response form opened normally" \
  --status passed \
  --effect inspected \
  --screenshot artifacts/response-form.png

python3 skill/hh-openclaw-agent/scripts/check_hh_submission_bundle.py \
  --manifest /tmp/hh-packet.json \
  --repo-root . \
  --out /tmp/hh-packet-check.json

python3 skill/hh-openclaw-agent/scripts/render_hh_submission_report.py \
  --manifest /tmp/hh-packet.json \
  --out /tmp/hh-packet-report.md
```

Quote URLs with query strings when running the example in `zsh`.
The markdown report is redacted by default. Use `--include-sensitive` only when you intentionally need a full export.

## What It Covers

- one machine-readable application packet for a single `hh.ru` response flow
- evidence-backed execution steps for review, browser interaction, submit, and outcome capture
- bundle validation for approval-gate mistakes, missing failure detail, and absolute artifact paths
- a shareable markdown report for review, debugging, or job-funnel records, with sensitive fields redacted by default

## Included

- `skill/hh-openclaw-agent/SKILL.md`
- `skill/hh-openclaw-agent/agents/openai.yaml`
- `skill/hh-openclaw-agent/scripts/init_hh_application_packet.py`
- `skill/hh-openclaw-agent/scripts/append_hh_execution_step.py`
- `skill/hh-openclaw-agent/scripts/check_hh_submission_bundle.py`
- `skill/hh-openclaw-agent/scripts/render_hh_submission_report.py`

## Use Cases

- prepare a vacancy-specific packet before opening the live `hh.ru` browser flow
- keep one auditable record of what was approved, what was submitted, and what blocked the run
- debug an `hh.ru` response flow with screenshots and exact browser steps
- hand a live application run to another operator without losing the context

## License

MIT

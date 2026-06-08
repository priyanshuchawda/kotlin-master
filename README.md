# Kotlin Master Skill

This repo publishes one reusable `skills.sh` skill for modern Kotlin and Android guidance:

- `kotlin-master` in `skills/kotlin-master/SKILL.md`

## Install from GitHub

```bash
npx skills add priyanshuchawda/kotlin-master --skill kotlin-master
```

## Test locally

```bash
npx skills add . --list
npx skills use . --skill kotlin-master
npx skills add . --skill kotlin-master -y
```

## Notes

- `skills.sh` discovers skills from repository paths such as `skills/<skill>/SKILL.md`.
- `skills.sh.json` only affects grouping/display on the `skills.sh` repo page.
- Visibility on `skills.sh` depends on the repo being installed or seen by the telemetry service, and page updates can lag behind cache refresh.

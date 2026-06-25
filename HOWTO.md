# eyeROV CI/CD Workflow Templates

## How to add CI to a new driver repo

### 1. Apply the templates
Go to your driver repo → **Actions → New workflow** → scroll to **"By eyerov organization"**

Apply these three:
- **eyeROV Python Driver CI** (for Python/pytest drivers)
  OR **eyeROV C++ Driver CI** (for C++/gtest drivers)
- **eyeROV Auto Release**
- **eyeROV Release**

### 2. Replace the placeholder
In all three generated files, search for `your_package_name` and replace
with your actual package name (e.g. `seatrac_usbl_driver`).

### 3. Create requirements.txt (Python drivers only)
In the repo root, create `requirements.txt` with your pip dependencies:
```
nucleus_driver==1.7.6
some-other-lib==2.0.0
```

The CI reads this automatically. `pytest` and `pytest-cov` are already installed.

### 4. Secrets — nothing to do
`ROS2_RELEASE_TOKEN` is already set as an org-level secret.
Every repo in the eyerov org uses it automatically.

---

## Commit prefix conventions

| Prefix | Version bump | Example |
|---|---|---|
| `Feat!:` or `Fix!:` | **major** `v1.0.0 → v2.0.0` | `Feat!: redesign driver API` |
| `Feat:` | **minor** `v1.0.0 → v1.1.0` | `Feat: add water track publisher` |
| `Fix:` `Tweak:` `Perf:` | **patch** `v1.0.0 → v1.0.1` | `Fix: handle empty packet` |
| `Chore:` `Test:` `Docs:` | **none** | `Chore: update README` |

---

## Coverage gate
Default is **80% line coverage**. To bypass for a critical hotfix,
add the label `emergency-hotfix` to the PR.

---

## How the three workflows connect

```
Developer merges PR to main
↓
Auto Release runs — reads commit prefixes → pushes v1.2.3 tag
↓
Release runs — generates changelog → creates GitHub Release
           → opens bump PR in eyerov_payloads
```

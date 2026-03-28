# Schema Registry (Multi-Module)

This repository manages Avro schemas for our distributed systems, organized by business domain. It uses an automated CI/CD pipeline to compile, test, version, and deploy schema JARs to our private Maven registry.

## 📁 Project Structure

* `flightops-schemas/`: Schemas for Flight Operations (e.g., Gate Changes, ETD).
* `notification-schemas/`: Schemas for Notifications (e.g., Email Requests, SMS Requests).
* `pom.xml`: The Parent POM managing shared dependencies and build logic.

## 🚀 How to Add or Update Schemas

1.  **Create a Branch:** `git checkout -b feat/add-new-schema`
2.  **Add/Edit .avsc files:** Place your schemas in the appropriate module's `src/main/avro/` directory or create a new module for a different domain.
3.  **Run Local Build:** Verify your changes locally:
    ```bash
    mvn clean package -Drevision=1.0.0-LOCAL -pl <module-name> -am
    ```
4.  **Submit a PR:** Use the **Conventional Commits** format for your PR Title (see below).

## 🏷️ Automated Versioning (Conventional Commits)

Our GitHub Action calculates the next version number based on the **PR Title** when merged to `main`. 

| PR Title Prefix | SemVer Impact | Example |
| :--- | :--- | :--- |
| `fix: ...` | **Patch** (Bug fix) | `1.0.1` → `1.0.2` |
| `feat: ...` | **Minor** (New feature) | `1.0.2` → `1.1.0` |
| `feat!: ...` | **Major** (Breaking change) | `1.1.0` → `2.0.0` |
| `fix!: ...` | **Major** (Breaking fix) | `1.1.0` → `2.0.0` |

> **Note:** Including `BREAKING CHANGE` in the PR description will also trigger a **Major** version bump.

## 📝 Changelogs
Each module maintains its own `CHANGELOG.md`. The CI bot automatically appends the PR title and version number to the relevant module's changelog upon merging.

## 🛠️ Local Development Tips

### Requirements
* **Java 25**
* **Maven 3.6.3+**

### Useful Commands
* **Generate Java Classes:** `mvn generate-sources`
* **Run Schema Tests:** `mvn test -pl <module-name>`
* **Install to Local .m2:** `mvn clean install -Drevision=9.9.9-LOCAL`
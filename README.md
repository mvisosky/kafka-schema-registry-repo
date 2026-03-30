# Kafka Schema Registry (Module Per Domain)

This repository manages Avro schemas for Kafka messaging, organized by business domain. It uses an automated CI/CD pipeline to compile, test, version, and deploy schema JARs to a Maven registry.

## 📁 Project Structure

* `pom.xml`: The Parent POM managing shared dependencies and build logic.
* `*-schemas/`: Avro and JSON Schemas for a particular business domain or team. Directory includes automatically managed CHANGELOG and pom.xml files for the module.

## 🚀 How to Scaffold a New Schema Module

1. **Run Scaffold Action** https://github.com/mvisosky/kafka-schema-registry-repo/actions/workflows/scaffold-module.yaml
2. **Provide Inputs**
   - Module Name (must end in `-schemas`) e.g. `flightops-schemas`
   - Short Description for Maven POM
3. **Check out the new branch associated with the generated draft PR**
4. **Add your .avsc and/or .json files:** Place your schemas in the `src/main/avro/` or `src/main/json/` directories of your new module.
5. **Run Local Build:** Verify your changes locally:
    ```bash
    mvn clean package -Drevision=0.0.0-LOCAL -pl <module-name> -am
    ```
6. **Commit changes and convert the draft PR to a PR:** Use the **Conventional Commits** format for your PR Title (see below).

## 🚀 How to Add or Update Schemas

1.  **Create a Branch:** `git checkout -b feat/add-new-schema`
2.  **Add/Edit .avsc and/or .json files:** Place your schemas in the module's `src/main/avro/` or `src/main/json/` directories.
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
* **Java 11**
* **Maven 3.6.3+**

### Useful Commands
* **Generate Java Classes:** `mvn generate-sources`
* **Run Schema Tests:** `mvn test -pl <module-name>`
* **Install to Local .m2:** `mvn clean install -Drevision=0.0.0-LOCAL`
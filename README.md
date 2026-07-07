Project Summary: CI/CD Pipeline with SonarQube + Docker
Objective: Build an automated CI/CD pipeline that tests, analyzes, containerizes, and deploys an application on every GitHub commit, with Jenkins secured via role-based access.
Stack: GitHub, Jenkins (Declarative Pipeline), Python/Flask + Pytest, SonarQube, Docker, DockerHub, Ubuntu Server.
Pipeline Flow:
GitHub push → Webhook triggers Jenkins
  → Build & Test (venv + Pytest)
  → SonarQube code analysis
  → Quality Gate (fails build if not met)
  → Docker image build
  → Push to DockerHub
  → Deploy container
Key Implementation Points:

GitHub webhook auto-triggers builds on every push — no manual intervention.
Pytest runs inside an isolated Python virtual environment, with results reported to Jenkins.
SonarQube scans code quality; waitForQualityGate(abortPipeline: true) halts the pipeline if the quality gate fails.
Docker image is built, tagged with build number + latest, and pushed to DockerHub using credentials stored securely in Jenkins (never hardcoded).
App is deployed by running the freshly pushed image as a container on port 5000.
Jenkins secured with Role-Based Authorization: an admin role (full access) and a restricted developer role (build/read only), assigned to separate accounts — demonstrating least-privilege access.

Challenges Resolved:

Missing DockerHub credential ID → added Username/Password credential with access token.
Corrupted venv from a failed build → added cleanup step (rm -rf venv).
SonarQube scanner binary missing → configured auto-install via Jenkins Global Tools.
DockerHub push failed due to a read-only token → regenerated token with Read/Write scope.

Outcome: A single git push now fully automates build, test, quality analysis, containerization, registry publishing, and deployment — with Jenkins access locked down by role.

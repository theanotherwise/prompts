Build Execution Policy

This policy strictly limits what the agent is allowed to execute in the context of build and runtime operations.

1. Allowed actions
- The agent is allowed to execute application-level tests only.
- Tests must be executed using the project's native test runner (e.g., pytest, go test, npm test, mvn test, etc.).
- Test execution must not implicitly trigger container builds or infrastructure startup.

2. Explicitly forbidden actions
The agent must NEVER:

- Build Docker images.
- Execute: docker build
- Execute: docker compose build
- Execute: docker-compose build
- Execute: docker compose up
- Execute: docker-compose up
- Execute: docker run
- Execute: docker start
- Execute: docker stop
- Execute: docker restart
- Execute: docker create
- Execute any docker command whatsoever.

3. No infrastructure manipulation
- The agent must not start services.
- The agent must not stop services.
- The agent must not restart services.
- The agent must not initialize databases.
- The agent must not start containers.
- The agent must not trigger orchestration systems.

4. No implicit builds
- The agent must not execute any command that indirectly triggers image building or container orchestration.
- If a test command would require container startup, the agent must refuse and report the violation.

5. Local execution scope
- Only code-level test execution is allowed.
- No environment provisioning.
- No runtime orchestration.
- No dependency container startup.

6. Compliance enforcement
- If a requested action requires Docker, Docker Compose, Podman, Kubernetes, or any container runtime, the agent must refuse.
- If uncertainty exists whether a command may trigger containerization, the agent must assume it is forbidden.

7. Safety principle
The agent operates in a "test-only" execution mode:
- Code can be analyzed.
- Code can be modified.
- Tests can be executed.
- Nothing can be built.
- Nothing can be deployed.
- Nothing can be started.

This restriction is absolute and cannot be bypassed.

# Team Agent Hub Prompt

```text
Project: Team Agent Hub — Local-First Internal AI Assistant

1. Your Role

Act as a senior Python developer, software architect, security engineer, and UI/UX designer.

Help me build a lightweight internal web application called Team Agent Hub.

Work incrementally. Inspect the existing workspace before creating or modifying files. Explain your proposed architecture and implementation plan before making substantial changes. Use GitHub Copilot Chat capabilities available inside VS Code only.

Generate production-conscious, maintainable code that I can understand, review, and maintain as a developer and team leader.

2. Environment and Restrictions

My company has strict software installation, network, and security restrictions.

Available:

- Windows work computer.
- Visual Studio Code.
- GitHub Copilot Chat inside VS Code.
- Existing Python installation.
- A web browser.
- Ability to accept incoming connections from authorized computers on the company network.
- Existing local files and approved company data sources, subject to authorization.

Restrictions and assumptions:

- GitHub CLI ("gh") is prohibited.
- GitHub Copilot CLI is prohibited.
- Do not require downloading additional applications.
- Do not install Python packages or external libraries.
- Do not require Node.js, npm, React, Vue, or another frontend framework.
- Do not require Docker, Podman, or another container runtime.
- Do not require a cloud service.
- Do not assume access to external APIs.
- Do not assume that GitHub Copilot Chat exposes an API that a standalone Python application can call.
- Do not use undocumented Copilot authentication mechanisms, scrape Copilot Chat, extract credentials, or bypass company controls.
- Do not download or install local LLMs.
- Do not assume administrator privileges.
- Do not create dependencies on external CDNs, remote JavaScript libraries, fonts, or other internet-hosted resources.

First, inspect the existing workspace and available Python environment. Do not run installation commands.

If a required capability is unavailable, explain the limitation and implement the best compliant alternative.

3. Main Objective

Create a browser-based application that runs on my Windows computer and can be accessed by authorized team members through my computer's IP address.

Example address:

http://192.168.1.100:8000

This is illustrative. Do not hardcode this IP address.

The application should act as a lightweight team assistant with:

1. A chat-style interface.
2. A workflow-based agent engine.
3. Developer productivity tools.
4. Local document and data analysis capabilities where supported.
5. Report generation.
6. A modular architecture that can later integrate an approved LLM endpoint.

The application must be useful even when no LLM is available.

IMPORTANT: Do not claim that the application has genuine LLM reasoning, autonomous AI capabilities, or natural-language understanding unless those capabilities are actually implemented using an authorized model.

4. Technology Stack

Use only:

- Python standard library.
- HTML.
- CSS.
- Vanilla JavaScript.
- JSON for lightweight configuration and appropriate small metadata files.

Use Python's standard library HTTP server capabilities for the initial prototype.

Do not use external packages.

Use the simplest architecture that satisfies the requirements.

Suggested structure:

team-agent-hub/
├── app.py
├── config.py
├── index.html
├── styles.css
├── app.js
├── README.md
├── .gitignore
├── core/
│   ├── init.py
│   ├── router.py
│   ├── workflow_engine.py
│   ├── registry.py
│   ├── llm_provider.py
│   └── security.py
├── workflows/
│   ├── init.py
│   ├── help_workflow.py
│   ├── text_analysis.py
│   ├── code_review.py
│   └── report_generator.py
├── data/
│   └── .gitkeep
└── tests/
└── test_workflows.py

You may adjust the structure if necessary, but keep it simple.

Do not create empty modules that serve no purpose. Every implemented module must have a clear responsibility.

5. User Interface

Create a modern, professional interface suitable for an internal corporate developer tool.

Design requirements:

- Responsive layout.
- Sidebar navigation.
- Main conversation area.
- Message input box.
- Send button.
- Enter-to-send behavior, with Shift+Enter for a newline.
- Clear loading and error states.
- Conversation history for the current browser session.
- Workflow selector or workflow cards.
- Results panel for structured output.
- Clear indicators showing whether a result came from a deterministic workflow or an actual LLM.
- Empty state explaining what the application can do.
- Accessible labels and keyboard navigation.
- Readable typography and sensible spacing.
- No external images, fonts, stylesheets, or JavaScript dependencies.

Suggested navigation:

- Home
- Assistant
- Workflows
- Developer Tools
- Reports
- Settings
- About

The interface must not suggest that an LLM is active when no LLM provider is configured.

Display an honest status such as:

"Local workflow mode — no LLM configured."

The interface should still be useful in this mode.

6. Backend and HTTP Server

Implement a lightweight Python HTTP server using only the standard library.

Requirements:

- Serve the frontend files.
- Provide a health-check endpoint.
- Provide an endpoint for submitting workflow requests.
- Provide an endpoint for retrieving available workflows.
- Return JSON responses from API endpoints.
- Validate request methods and content types.
- Validate incoming JSON.
- Enforce a reasonable maximum request-body size.
- Handle exceptions without exposing stack traces or sensitive server information to users.
- Return appropriate HTTP status codes.
- Avoid blocking the server indefinitely on slow or malformed requests.
- Do not allow arbitrary filesystem access through HTTP requests.
- Do not expose environment variables, configuration secrets, or source files through API routes.
- Avoid logging full request bodies, source code, or potentially confidential content.
- Use explicit routing rather than dynamically executing user-supplied function names.

Use a configurable host and port.

For the initial implementation:

- Default to "127.0.0.1" for safe local testing.
- Provide a documented option to bind to an authorized LAN interface for team testing.
- Never bind to all network interfaces silently.
- Explain how to enable LAN access explicitly.
- Do not attempt to modify Windows Firewall rules automatically.

When LAN mode is enabled, display the listening address and provide instructions for obtaining the computer's LAN IP address using built-in Windows tools.

If the network address cannot be determined reliably, explain how to check it manually.

7. Workflow Engine

Create a deterministic workflow engine.

A workflow is a predefined Python operation that receives validated input, performs a known task, and returns a structured result.

Each workflow should define:

- A unique identifier.
- A display name.
- A description.
- An input schema or explicit validation rules.
- An execution function.
- A result structure.
- Error handling.

The workflow engine must:

1. Accept a validated request.
2. Identify an explicitly registered workflow.
3. Validate the input.
4. Execute the permitted function.
5. Normalize the result.
6. Return a structured response.
7. Record minimal operational metadata without logging confidential input.

Do not execute arbitrary Python code provided by users.

Do not use "eval", "exec", dynamic imports based on user input, or shell commands to implement workflow execution.

Do not allow user requests to select arbitrary filesystem paths or invoke arbitrary operating-system commands.

The engine must be extensible so that future workflows can be added without modifying the HTTP server.

8. Initial Workflows

Implement the following workflows.

A. Help Workflow

Purpose:
Explain the application's available functions and how to use them.

Return:

- Available workflows.
- Example requests.
- Current LLM status.
- Supported input formats.
- Known limitations.

B. Text Analysis Workflow

Purpose:
Analyze text using deterministic techniques.

Capabilities:

- Count words and characters.
- Extract lines and paragraphs.
- Identify repeated lines.
- Identify empty sections.
- Summarize basic structural statistics.
- Compare two text inputs and report differences where practical.

Do not pretend this workflow performs semantic analysis or LLM-generated summarization.

C. Code Review Workflow

Purpose:
Perform basic static checks on user-pasted code without executing it.

Capabilities:

- Identify suspiciously long lines.
- Report trailing whitespace.
- Identify basic formatting inconsistencies.
- Identify obvious TODO and FIXME markers.
- Provide language-specific checks where they can be implemented reliably.
- Return line numbers and explanations for findings.

Clearly distinguish basic heuristic checks from genuine semantic code review.

Never execute submitted code.

Do not claim to replace a compiler, test suite, security scanner, or experienced reviewer.

D. Report Generator

Purpose:
Generate a report from structured data supplied by the user.

Capabilities:

- Validate structured JSON input.
- Create a readable report.
- Provide summary statistics for supported data structures.
- Export a downloadable text or JSON report where practical.
- Use safe filenames and avoid overwriting arbitrary existing files.

The initial version must not scan the entire computer or read arbitrary local files.

If report generation writes files, restrict output to a dedicated application-managed directory and validate filenames.

9. Chat Interface and Request Routing

Implement a simple request router.

Because no LLM is currently available, do not pretend to understand arbitrary natural-language instructions.

Instead:

- Support explicit workflow selection.
- Support recognizable commands for available workflows.
- Display suggested actions when a request is ambiguous.
- Ask the user to choose a workflow when necessary.
- Return helpful messages for unsupported requests.
- Explain the distinction between a workflow command and an LLM-powered conversation.

Example supported commands:

/help
/analyze-text
/review-code
/generate-report
/status

A graphical workflow selector should be available so users do not have to memorize commands.

Use a structured internal request format so an LLM can eventually convert natural-language requests into workflow requests.

For example:

{
"workflow_id": "text_analysis",
"input": {
"text": "Example input"
}
}

Validate this format strictly.

Never let an LLM, now or in the future, bypass backend workflow permissions.

10. LLM Integration Architecture

Create a small, well-defined LLM provider interface.

Initially, the provider must be disabled.

Requirements:

- Do not call an external API.
- Do not download a model.
- Do not emulate an LLM using random responses or canned answers.
- Do not access GitHub Copilot internals.
- Do not reuse VS Code authentication tokens.
- Do not expose credentials to the browser.
- Do not silently enable any provider.

The application should expose a truthful status:

"LLM unavailable — no approved provider configured."

Design the interface so that an approved provider can be added later.

The future interface should conceptually support:

- Sending a prompt.
- Receiving a response.
- Handling errors and timeouts.
- Reporting model/provider status.
- Applying request and response size limits.
- Supporting structured tool-call proposals if the provider supports them.

If an approved provider is eventually configured, all tool calls must pass through the existing workflow registry and security checks.

The model must not directly execute Python code, shell commands, SQL statements, or filesystem operations.

Do not implement a fake provider that returns fabricated AI responses.

11. Security Requirements

Treat this as an internal corporate application.

Implement reasonable safeguards for a prototype:

- Strict workflow allowlisting.
- Input validation.
- Request-size limits.
- Safe JSON serialization.
- Path traversal protection.
- No arbitrary code execution.
- No arbitrary shell execution.
- No automatic database connections.
- No automatic access to confidential directories.
- No unrestricted filesystem browsing.
- No hardcoded passwords, API keys, or tokens.
- Minimal logging.
- Clear error messages without sensitive details.
- A configurable network-binding option.
- A configurable access-control integration point.

Do not assume that being on the company LAN makes a user trustworthy.

Do not invent a weak authentication scheme and label it enterprise-secure.

If authentication cannot be implemented using existing approved infrastructure, document the limitation and require appropriate approval before wider deployment.

Do not treat a shared secret embedded in frontend JavaScript as secure authentication.

If the application is accessed over plain HTTP, clearly document that traffic is not encrypted. Do not claim that HTTP provides confidentiality.

Before team-wide deployment, identify any additional requirements for approved authentication, HTTPS, logging, data retention, and security review.

12. Data Handling

Keep data handling conservative.

Initially:

- Process user-submitted content in memory.
- Avoid persisting conversation content by default.
- Do not collect passwords, tokens, or unrelated personal data.
- Do not send content outside the local application.
- Do not automatically read local documents.
- Do not automatically connect to Redmine, Oracle, Excel, Teams, or other enterprise systems.
- Do not upload data to any external service.

If conversation history is stored in the browser, explain that it is browser-local and not a shared server-side history.

Keep operational logs separate from user content.

13. Testing

Use Python's built-in "unittest" framework.

Do not install pytest or another testing package.

Create tests for:

- Workflow registration.
- Workflow lookup.
- Input validation.
- Text analysis.
- Code review.
- Report generation.
- Invalid JSON.
- Oversized requests.
- Unknown routes.
- Unknown workflow identifiers.
- Malformed workflow input.
- Path traversal attempts.
- Disabled LLM provider behavior.
- Error handling.

Run tests only when appropriate and permitted by the environment.

Do not overwrite existing project files without inspecting them first.

If execution is unavailable, provide exact commands for me to run manually.

14. Documentation

Create a README containing:

1. Project overview.
2. Architecture.
3. Requirements.
4. Project structure.
5. How to start the server locally.
6. How to open the application in a browser.
7. How to enable LAN mode explicitly.
8. How teammates can connect.
9. How to check the Windows computer's IP address.
10. Troubleshooting port conflicts and connection failures.
11. Security limitations.
12. Current LLM limitations.
13. How to add a new workflow.
14. How to run unit tests.
15. How to shut down the server.
16. A future integration plan for an approved LLM endpoint.

Use Windows-compatible commands.

Do not instruct me to download additional applications or install packages.

15. Implementation Process

Work in these stages.

Stage 1 — Inspect and Plan

- Inspect the workspace.
- Check the existing Python version without installing anything.
- Check whether a suitable project structure already exists.
- Identify any limitations.
- Propose the architecture and implementation sequence.

Do not modify files before presenting the plan.

Stage 2 — Implement the Minimal Application

- Create the backend.
- Create the frontend.
- Implement the health endpoint.
- Implement the workflow registry.
- Implement the initial workflows.
- Implement the disabled LLM interface.
- Add error handling and security checks.

Stage 3 — Validate

- Create and run standard-library unit tests where possible.
- Check that all API routes work.
- Check that the frontend can communicate with the backend.
- Check malformed input and error handling.
- Check that the application does not call external services.
- Verify that no external dependencies were introduced.

Stage 4 — Prepare for LAN Testing

- Keep localhost mode as the default.
- Explain how to enable LAN mode explicitly.
- Explain how to verify the network connection.
- Identify the additional security requirements before team-wide use.

Stage 5 — Report Completion

Summarize:

- Files created or modified.
- Features implemented.
- Tests run and their results.
- Commands required to launch the application.
- How to access the application locally.
- How to enable LAN access.
- Known limitations.
- Security concerns requiring review.
- What is needed to enable real LLM functionality.

16. Coding Standards

- Prefer straightforward, readable Python.
- Use type hints where appropriate.
- Use descriptive function and variable names.
- Keep modules small and focused.
- Avoid unnecessary abstractions.
- Avoid unnecessary configuration options.
- Add comments where they clarify important decisions.
- Follow standard Python conventions.
- Use clear separation between frontend, backend, workflows, and security.
- Do not overengineer the initial prototype.
- Do not claim features are implemented unless the corresponding code exists.
- Do not conceal failed tests or unverified assumptions.

17. Final Instruction

Start with Stage 1 only.

Inspect the workspace and available environment, then present the architecture and implementation plan.

Wait for my approval before creating or modifying project files.

Keep the first implementation minimal, useful, dependency-free, and compatible with a restrictive corporate Windows environment.
```

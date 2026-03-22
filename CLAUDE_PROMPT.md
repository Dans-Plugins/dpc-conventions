# GitHub Coding Agent Prompt: Align a DPC Plugin with DPC Conventions

Paste the prompt below into the GitHub Coding Agent to automatically bring a DPC plugin
repository up to the standards defined in the
[dpc-conventions](https://github.com/Dans-Plugins/dpc-conventions) repository.

---

## Prompt

```
Align this repository with the DPC (Dans Plugins Community) conventions defined at
https://github.com/Dans-Plugins/dpc-conventions.

Read each of the following convention documents in full before making any changes:

- README structure:        https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/README_STRUCTURE.md
- Documentation practices: https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/DOCUMENTATION_PRACTICES.md
- Contributing standards:  https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/CONTRIBUTING_STANDARDS.md
- Testing and CI:          https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/TESTING_AND_CI.md
- Release automation:      https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/RELEASE_AUTOMATION.md
- GitHub Copilot instructions:      https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/GITHUB_COPILOT_INSTRUCTIONS.md

Then perform the following steps:

1. Inspect the repository — read README.md, CONTRIBUTING.md, any existing docs/, and
   .github/workflows/ files to understand the current state.

2. Build a gap checklist comparing the repo against every requirement in the convention
   documents above.

3. Implement all required changes directly in the repository:
   - Update README.md to include all required sections in the correct order, following
     the templates in README_STRUCTURE.md.
   - Create or update CONTRIBUTING.md using the template in CONTRIBUTING_STANDARDS.md.
   - Create any missing documentation files (USER_GUIDE.md, COMMANDS.md, CONFIG.md,
     CHANGELOG.md) with appropriate starter content derived from the plugin's existing
     code and configuration.
   - Add or update .github/workflows/build.yml using the CI template in TESTING_AND_CI.md.
   - Add or update .github/workflows/release.yml using the template in RELEASE_AUTOMATION.md.
   - Create or update .github/copilot-instructions.md using the template in GITHUB_COPILOT_INSTRUCTIONS.md.
   - Make any other changes needed to satisfy the conventions.

4. Do not restate convention rules in commit messages or comments — reference the
   relevant convention document URL instead.

Keep changes minimal and focused on closing the identified gaps. Do not refactor
existing code or alter plugin behaviour.
```

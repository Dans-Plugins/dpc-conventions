# Claude Prompt: Align a DPC Plugin with DPC Conventions

Copy and paste the prompt below into Claude to get step-by-step guidance on bringing a DPC plugin repository up to the standards defined in the [dpc-conventions](https://github.com/Dans-Plugins/dpc-conventions) repository.

---

## Prompt

```
You are helping align a Dans Plugins Community (DPC) Minecraft plugin repository with the
standards defined in https://github.com/Dans-Plugins/dpc-conventions.

The conventions repository contains the following documents — read each one before proceeding:

- README structure:       https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/README_STRUCTURE.md
- Documentation practices: https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/DOCUMENTATION_PRACTICES.md
- Contributing standards:  https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/CONTRIBUTING_STANDARDS.md
- Testing and CI:          https://github.com/Dans-Plugins/dpc-conventions/blob/main/docs/TESTING_AND_CI.md

The plugin repository I want to align is: <INSERT GITHUB REPO URL HERE>

Please do the following:

1. Review the current state of the repository (README.md, CONTRIBUTING.md, and any other
   documentation files present).

2. Produce a gap analysis — a checklist that shows which conventions are already satisfied
   and which are missing or incomplete.

3. For each gap, provide the exact content (Markdown) needed to close it, ready to copy
   into the repository. Follow the templates in the conventions documents exactly.

4. List any files that need to be created from scratch (e.g. USER_GUIDE.md, COMMANDS.md,
   CONFIG.md) and provide starter content for each, based on the conventions.

5. Describe any CI/testing changes needed (e.g. adding a .github/workflows/build.yml) and
   provide the file content.

Do not restate the conventions — link back to the relevant section of the conventions
repository instead. Keep your output focused on the specific gaps in this plugin's repo.
```

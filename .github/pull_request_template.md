
# Clickup Task
*Include the ClickUp id/url*


# Summary
*Include a summary of the change and which issue is fixed. Please also include relevant motivation and context. List any dependencies that are required for this change.*


# Type of change

- [ ] New feature (non-breaking change which adds functionality)
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation or other change that does not affect functionality

# Developer Checklist:

- [ ] I have commented my code, including roxygen comments (R) or docstrings + type hints (Python)
- [ ] I have made corresponding changes to the documentation and README
- [ ] I have added and updated relevant tests, and these pass locally
- [ ] I have updated the CHANGELOG/NEWS.md, documenting these changes (if applicable)
- [ ] I have added log messages where appropriate (if applicable)
- [ ] Database connections are terminated at the end of a query
- [ ] No passwords or secrets appear in code or any files included in git (git history should be reverted if this occurs)
- [ ] I have updated `renv.lock`/`manifest.json`/`DESCRIPTION`/`NAMESPACE`/`requirements.txt` with any new dependencies
- [ ] My code follows the style guidelines of this project

# Deploying to Production (skip if this is not a PR to a production branch)
- [ ] I have performed UI tests on staging following guidelines provided [here](https://app.clickup.com/2346452/v/dc/27kem-5880/27kem-3325)
- [ ] Any sysadmins, users, or other stakeholders who should know about this deployment are aware of the deployment scheduled

***

# PR Reviewer Checklist
[*PR review tips*](https://app.clickup.com/2346452/v/dc/27kem-5880/27kem-2772)
- [ ] I have reviewed each line of code, checking for common issues/vulnerabilities listed [here](https://app.clickup.com/2346452/v/dc/27kem-5880/27kem-2772)
- [ ] I have run this code locally and it worked as expected
- [ ] I have run all tests locally and they pass

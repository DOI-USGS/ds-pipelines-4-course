When you're developing a shared-cache pipeline, here are some things to check periodically to ensure the health (efficiency) of your pipeline:

- [ ] Are your getters isolated in their own YAML file that's independent of the main pipeline?
- [ ] Are your line endings all Posix (LF) everywhere? **Everywhere??** Check the .gitattributes file and scan for base-R text-file-writing functions. **can we suggest a command to check for these automatically throughout the repo?**


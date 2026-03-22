# Git Command Reference

## `git commit`

### Empty commit

- When you want to create a commit without any changes, you can use the `--allow-empty` option. This is useful for documenting a point in history or triggering CI/CD pipelines without modifying the codebase.
- It is sometimes used in cases such as when you want to create an empty initial commit when creating a repository.

```bash
git commit --allow-empty -m "your commit message"
```

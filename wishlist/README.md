## reset to tag or commit keeping your current state as untracked or modified

```bash
git reset # [ref]
```

automatically stage the changes
```bash
git reset # [ref] --soft
```

undo
```bash
git reset --hard HEAD@{1}
```

## plan

never commit changes directly to the skeleton -- changes should only be made programmatically!

# Contributing
Contributing is easy, there are only a few things you should follow.

It is of my understanding that my formatting standards are not Java conventions, so I'll list the generalized rules below:
```
finals: SCREAMING_SNAKE_CASE
public: PascalCase
private/package-private/protected: camelCase
packages: lowercase
```

### Package Naming Conventions
When contributing an interface, it should be under the following package: `tomat.dev.bridge.full.original.package.IOriginalClassNameBridge`

For example, `dev.tomat.mojang.NameHistoryEntry` becomes `tomat.dev.bridge.dev.tomat.mojang.INameHistoryEntryBridge`.
Of course, having `dev.tomat` in there twice looks ugly, but other packages look better, I swear.

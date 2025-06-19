# Git Recycle Bin - Artwork Repository

This repository demonstrates the concept and benefits of **git-recycle-bin**, a approach for managing binary 
assets in Git repositories while maintaining the benefits of version control without the drawbacks of bloating 
the main source repository.

**This repository exists specifically to isolate binaries away from the main source repository, while still 
allowing the source repo to import them as a submodule.**

## Repository Layout Structure

```
.
├── grb-flow/                    # Process flow diagrams
│   ├── binaries-in-source.drawio.svg           (873KB) - How to manage binaries in source
│   ├── binaries-from-source-simple.drawio.svg (129KB) - Simplified git-recycle-bin flow
│   └── binaries-from-source.drawio.svg        (642KB) - Detailed git-recycle-bin flow
├── grb/                         # Git Recycle Bin assets
│   ├── vector/                  # Production-ready vector assets
│   │   ├── flat1-color-green.svg     (36KB) - Green themed logo
│   │   ├── flat1-color-dark.svg      (36KB) - Dark themed logo  
│   │   ├── flat1-bw-outline-b.svg    (37KB) - Black outline logo
│   │   ├── flat1-bw-outline-w.svg    (37KB) - White outline logo
│   │   └── flat1-bw.svg              (36KB) - Black & white logo
│   ├── draft-rasters/           # AI-generated draft images (26 files, ~1-3MB each)
│   │   ├── 3d - *.png           # 3D style variations
│   │   ├── flat - *.png         # Flat style variations
│   │   └── original.png         # Original reference image
│   └── environmental/           # Environmental/scene context images
│       └── scene - *.png        # Save the planet by build avoidance
└── artifactlabs/               # Umbrella organization
```

## What are .drawio.svg Files?

The `.drawio.svg` files are **Draw.io diagrams saved in SVG format**. Draw.io (now diagrams.net) is a popular 
diagramming tool that can export diagrams as SVG files with embedded metadata that allows them to be reopened 
and edited in Draw.io.

### Key characteristics:
- **Editable**: Can be reopened in Draw.io for modifications
- **Vector format**: Scalable without quality loss
- **Web-compatible**: Can be displayed directly in browsers and markdown
- **Metadata-rich**: Contains both visual content and editing information
- **Self-contained**: Single file contains the complete diagram

These files serve as both documentation and editable source files for the process flow diagrams explaining 
git-recycle-bin concepts.

## Binary Assets in Git: When and Why

### When Binaries in Git are Acceptable

Binaries are **appropriate in Git** when they meet these criteria:
- ✅ **Preserve forever**: Assets with long-term value
- ✅ **Rarely change**: Infrequent updates minimize repository bloat
- ✅ **Small enough**: File sizes that don't significantly impact clone times
- ✅ **Core to project**: Essential assets directly tied to source code

Examples: Small logos, icons, reference images, documentation diagrams.

### When Binaries Should be Delegated Elsewhere

Binaries should **NOT be in the main source repository** when:
- ❌ **Large files**: Significantly impact repository size and clone performance
- ❌ **Frequent changes**: Regular updates cause repository bloat
- ❌ **Retention policies needed**: Assets that should be cleaned up over time
- ❌ **Derived artifacts**: Generated content that can be recreated

Examples: AI-generated iterations, build artifacts, large media files, temporary drafts.

## The Git-Recycle-Bin Solution

**Git-recycle-bin** addresses the binary management challenge by using **separate Git repositories** for 
different asset categories. This approach provides:

### Benefits of Git-Based Binary Storage

1. **Git Host Compatibility**: Use any Git hosting service (GitHub, GitLab, Bitbucket, etc.)
2. **Reuse Authentication**: Same credentials and access controls as source code
3. **Efficient Wire Protocol**: Git's delta compression and sparse checkout capabilities
4. **Built-in Garbage Collection**: Git's native cleanup mechanisms
5. **Full Traceability**: Complete history and blame information
6. **No Vendor Lock-in**: Standard Git repositories, portable across providers
7. **Familiar Tooling**: Use standard Git commands and workflows

### Architecture Philosophy

```
Source Repository (lean)     →    Binary Builds Repository (dedicated)
├── Source code              →    ├── Production assets
├── Small essential assets   →    ├── Draft iterations  
└── Documentation           →    ├── Generated content
                             →    └── Large media files
```

This **segregation of source from derived-artifact binaries** ensures:
- **Clean source history**: Main repository stays focused and fast
- **Specialized storage**: Binary repositories optimized for asset management
- **Independent lifecycles**: Different retention and cleanup policies
- **Scalable architecture**: Add more binary repositories as needed

### Implementation in This Repository

This repository demonstrates the concept with:
- **`grb-flow/`**: Process documentation (would also be appropriate for source repo)
- **`grb/`**: Actual binary assets that are large and plenty enough to keep away from source repo

A main source repository could reference the stable binary repository via:
```bash
git submodule add https://github.com/org/project-assets.git assets
```

This allows the source repository to:
- ✅ Import specific asset versions via commit hash - not a floating _latest_ from some central storage
- ✅ Keep its own history clean and fast
- ✅ Maintain reproducible builds
- ✅ Benefit from Git's efficient protocols
- ✅ Benefit from Git's submodule's `update=none` or `shallow`

## Binary Policies: Like Branching Policies, But for Assets

Just as repositories have **branching policies** (main branch protection, feature branch workflows, etc.), 
teams should establish **binary policies** that define how different types of assets are managed across 
repositories.

### Binary Policy Framework

Different binary repositories should have different policies based on their content and usage patterns:

#### 🔒 **Infinite Retention + Low Churn** (This Repository)
- **Policy**: Permanent storage, minimal changes
- **Integration**: ✅ **Safe for submodule import**
- **Characteristics**: 
  - Assets are preserved indefinitely
  - Changes are infrequent and deliberate
  - History remains stable and reliable
- **Examples**: Final logos, brand assets, documentation diagrams
- **Submodule Safety**: Excellent - consuming repositories can safely depend on stable references

#### ⏰ **Expiring + High Churn** (Not Suitable for Submodules)
- **Policy**: Orphan branches with expiration, frequent iterations
- **Integration**: ❌ **NOT safe for submodule import**
- **Characteristics**:
  - Branches may be deleted/expired
  - Frequent commits and iterations
  - History may be rewritten or pruned
- **Examples**: AI-generated drafts, build artifacts, temporary experiments
- **Submodule Risk**: Dangerous - references may become invalid, breaking consuming repositories

### This Repository's Binary Policy

This artwork repository follows an **infinite retention + low churn** policy:

```
Binary Policy: git-recycle-bin-artwork
├── Retention: Infinite (assets preserved forever)
├── Churn Rate: Low (changes are rare and intentional)  
├── Branch Stability: High (no orphan branches, no expiration)
├── Submodule Safety: ✅ SAFE
└── Integration Pattern: Designed for submodule import
```

**Why this works for submodule import:**
- Commit hashes remain valid indefinitely
- No risk of orphan branch deletion
- Stable references that won't break consuming repositories
- Predictable history that supports reliable builds

## Conclusion

Git-recycle-bin enables teams to maintain the benefits of Git-based asset management while avoiding the 
performance penalties of large binary files in source repositories. By strategically separating concerns 
across multiple Git repositories, teams can achieve optimal performance, maintainability, and developer 
experience.
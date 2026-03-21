# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a [Zenn](https://zenn.dev) content repository for managing and publishing articles to Zenn, a Japanese tech blogging platform. Content is written in Markdown and synced via GitHub integration.

## Commands

```bash
# Start local preview server (opens at http://localhost:8000)
bunx zenn preview

# Create a new article
bunx zenn new:article

# Create a new book
bunx zenn new:book

# List articles
bunx zenn list:articles
```

## Article Frontmatter

Every article in `articles/` requires this frontmatter:

```yaml
---
title: "記事タイトル"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["topic1", "topic2"] # max 5 topics
published: true # false = draft
---
```

Article filenames are auto-generated slugs (e.g., `3d610335fa0273.md`). Do not rename them as Zenn uses the filename as the article slug/URL.

## Content Structure

- `articles/` — Individual article files (one `.md` per article)
- `books/` — Book directories (each book is a subdirectory with chapter files)

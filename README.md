# kms0805.github.io

Minsung Kim's academic homepage — https://kms0805.github.io

Built with [Jekyll](https://jekyllrb.com/) on the [al-folio](https://github.com/alshedivat/al-folio) theme
and deployed to GitHub Pages by the `Deploy site` workflow on every push to `master`.

## Local development

```bash
bundle install
bundle exec jekyll serve   # http://localhost:4000
bundle exec jekyll build   # one-off build into _site/
```

Formatting is checked in CI:

```bash
npx prettier . --check     # --write to fix
```

## Where content lives

| Path                                | Contents                                                            |
| ----------------------------------- | ------------------------------------------------------------------- |
| `_pages/about.md`                   | Front page: profile, bio                                            |
| `_bibliography/papers.bib`          | Publications (`selected={true}` surfaces a paper on the front page) |
| `_news/announcement_N.md`           | News items                                                          |
| `_data/cv.yml`                      | Education and experience                                            |
| `_sass/`, `_layouts/`, `_includes/` | Styling and templates                                               |

See [CLAUDE.md](CLAUDE.md) for the conventions these files follow.

## License

Theme licensed under [MIT](LICENSE). Site content © Minsung Kim.

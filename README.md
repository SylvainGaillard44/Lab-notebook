# Sylvain Gaillard's lab notebook

Research notes, experimental observations, protocols, and analyses.

## Browse the notebook

- [Dated entries](_posts/)
- [New entry template](templates/entry-template.md)
- [Images](assets/images/)

## Add a new entry on GitHub

1. Open the [entry template](templates/entry-template.md), select **Code** to see its Markdown source, and copy the contents.
2. Open the [`_posts` folder](_posts/) and choose **Add file → Create new file**.
3. Name the file `YYYY-MM-DD-short-title.md`, using the date of your entry (for example, `2026-10-20-culture-observations.md`).
4. Paste the template, replace the title and prompts, and remove any sections you do not need.
5. Use **Preview** to check your entry, then **Commit changes** to save it.

To revise an entry, open it and select the pencil icon. GitHub keeps the history of saved changes.

## Organization

Entries use the `_posts` folder and dated filenames used by Jekyll. For now, this notebook is read directly on GitHub; no website deployment is configured.

Keep the small metadata block between `---` lines at the top of each entry. It makes entries easier to reuse with Jekyll later. No coding is needed to write the notes.

To include a photograph, upload it to [`assets/images`](assets/images/) and insert an image link in your entry:

```markdown
![Description of the photograph](../assets/images/my-photo.jpg)

*Figure 1. Description. Photo: photographer's name.*
```

Convert `[file]` to JTB. Invoke the `jtb-conversion` and `jtb-layout` skills
before starting.

Write the converted output to `index.html` if it contains only the default
Vite boilerplate (no meaningful content beyond the module script tag). If
`index.html` has real content, use `dev.html` instead, applying the same
check. If both files have real content, stop and ask. Remove the Tailwind CDN
script and replace it with `<script type="module" src="/main.js"></script>`.

Record actionable gaps in `jtb-conversion-notes.md` in the project root.
Create the file if it does not exist.

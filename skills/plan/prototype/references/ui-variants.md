# UI variants

Several **radically different UI variants** on one route, switched from a floating bar. The user flips between them in the browser, picks one or takes parts from several, and the rest is thrown away.

## Where the variants go

A variant is far easier to judge next to the rest of the app, with the real header, sidebar, data and density. On an empty route, every variant looks fine.

1. **An existing page (the default)**: Render the variants on the page's own route, chosen by a `?variant=` search parameter. The data fetching, parameters and auth stay as they are, and only the rendered part swaps. Something that would live inside an existing page, such as a new section, card or step, mounts inside that page too.
2. **A new page (the last resort)**: Only when nothing existing could host it, such as an entirely new top-level surface. Follow the project's routing convention, put `prototype` in the path, and use the same `?variant=` switch.

## 1. Pick the number of variants

Default to **three**, and stop at five, because beyond that they stop being radically different. Write the plan in one line beside the question, such as "Three variants of the settings page, switched by `?variant=`, on the existing `/settings` route."

## 2. Make them radically different

Hold each variant to the page's purpose and the data it has, use the project's component library and styling, and export it under a clear name, such as `VariantA`.

The variants differ in **structure**: layout, information hierarchy and primary action. Variants that differ only in colour or copy are a tweak, not a prototype. When two come out too similar, redo one with an explicit constraint, such as "no card grid".

Share little code between them. A shared header is fine, but a shared layout defeats the point, because each variant is free to throw out the layout.

## 3. Build the floating switcher

A small bar, fixed at the bottom centre of the screen:

1. **Left arrow**: The previous variant, wrapping around.
2. **Label**: The current variant's key, and its name when it has one, such as `B: Sidebar layout`.
3. **Right arrow**: The next variant, wrapping around.

The arrows update the URL through the framework's router, so a variant can be shared and survives a reload. The `←` and `→` keys cycle too, except while an `<input>`, a `<textarea>` or a `[contenteditable]` element has focus. Style the bar so it clearly isn't part of the design being judged, such as a high-contrast pill with a light shadow.

## 4. Capture and promote it

Hand over the URL and the variant keys. The useful feedback is usually "the header from B with the sidebar from C", which is the design the user actually wants. Capture that answer, which variant or mix and why, as the skill describes.

When the winner is built for real, rebuild it properly in the real page. The variants were written without tests or error handling, and all of them stay on the prototype branch.

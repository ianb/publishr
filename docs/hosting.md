# Hosting

Mozilla could engage with its mission by creating a hosting platform that serves as the nexus for open source publishing tool innovation.

## Motivation

Some of the motivation is described in [this blog post](http://www.ianbicking.org/blog/2019/01/we-need-open-hosting-platforms.html), but to paraphrase:

1. Open source publishing tools aren't usable by the public, because only _services_ can exist on the web.
2. Hosting is technically very easy and cheap.
3. Hosting is socially complicated and requires attention and maintenance (abuse monitoring, responding to legal inquiries, billing, etc.). It can't be done reliably by an individual.

There are open source developers that wants to make blog software, or an interesting image editor, or a story builder, or TIL collector, etc. Yet innovation is stifled because those developers cannot find an audience, and cannot provide a complete experience for average users.

## The hosting

Mozilla would provide hosting, backed by a Firefox Account (but otherwise not directly connected to Firefox).

We would provide a library that would mediate different kinds of access. For instance, imagine I am building an image editor:

1. The "application" is a URL. It can have a dynamic or static backend. Let's say `https://exampleuser.github.io/image-editor`
2. It would include a library (e.g., `https://hosting.firefox.com/client-library.js`), and maybe an optional local-UI library (not strictly part of the protocol, but it would make it easy to implement the expected basic UI).
3. A user would visit the site (on github.io in this case) and click a button to both log in and authorize the application to write. At the time of login the user could say the application only has temporary access (content will automatically delete after a couple days unless the user goes back and changes it), and whether content is public, etc.
4. Applications will create "documents" which have UUIDs, titles, and can contain several resources (HTML, images, audio, etc). These can be managed inside or outside the application.
5. The application would make CORS fetch requests (maybe WebSocket?) to actually save files, get file listings, etc.
6. The application can link to the management UI for the documents created by the application (maybe the interface would be iframe'd in), which lets a user "publish" a document at a particular URL, share it with specific people, create a shareable link.
7. If content is not published, then it would typically be encrypted at rest.
8. Applications can also upload JavaScript with their documents. While we might cache and share JavaScript (with a kind of content-addressable scheme), we wouldn't allow hotlinking to external scripts.
9. As a later optimization, we might allow some JavaScript to run on the server side, effectively the React pattern where the DOM might be updated on the server and then hydrated on the client, though it should be able to hydrate with no server-side processing as well.

## Management

Opening up personal hosting to third-party applications has many potential issues. What if an application spams your documents with excessive content? What if something becomes corrupted by the application? Or a developer uploads a hostile version of the application?

Many kinds of core functionality would not be part of applications, but of the system:

1. Per-application or per-document quotas
2. Time limits on content by documents or applications, or "ask me later" functionality.
3. For published content, specify your own ToS. E.g., you could ask us to take down or limit your content if anything pornographic appeared. So if we allow, say, Disqus commenting, then we could automatically disable that commenting if we detect problematic content, with notification but no requirement of user response.
4. Sharing, ACLs. If we allow sharing, maybe we implement some kind of in-hosting commenting separate from the application.
5. Documents would be versioned. Potentially we could require that the applications themselves be fully static and versioned, so if you make a document you know you can always get a version of the application that can edit that application. Permanent non-expiring applications.
6. Because documents are self-contained, they can be backed up, archived, encrypted, moved around.

## Related work

* https://remotestorage.io/
* https://neocities.org/
* https://neocities.org/api
* https://www.netlify.com/

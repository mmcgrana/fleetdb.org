---
layout: page
title: "Docs: Query Reference : auth"
docs_tab: "qr"
---

auth
-----

    ["auth", <password>]
    => <"auth-accepted"|"auth-rejected"|"auth-unneeded">

Authenticate with server using the given `password`. If the server is requiring with authentication because an `-x <password>` command line option was used, returns `"auth-accepted"` or `"auth-rejected"` according to whether the provided `password` matches the expected `password`. Otherwise returns `"auth-unneeded"`.
  
Note that the `auth` query must be the first sent by the client after connecting to the server if authentication is required. If the client sends a non-`auth` query in this case, the server will respond with `"auth-needed"` and terminate the TCP session.

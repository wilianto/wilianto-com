---
title: "[TIL] HTTP Method Name in Lowercase"
date: 2017-11-01 09:52:00 Z
categories:
- til
layout: post
meta_keywords: http method name, rfc7231
meta_description: Lowercase HTTP method name make me stuck getting bad request for an hour
comments: true
---

Today I learn that HTTP method name in HTTP request must be in upper case.

I've been working with web base apps for arround 7 years, but the "stupid" think is I don't aware that HTTP method does not support lowercase letter. I just realize it today, after stuck in arround an hour with my pairing partner. We get 400 Bad Request.

After searching, we get information about this in RFC 7231. Here is the point we get for HTTP method standard.

<div class="highlighter-rouge">
  <pre class="highlight">
    <code>
HTTP was originally designed to be usable as an interface to
distributed object systems.  The request method was envisioned as
applying semantics to a target resource in much the same way as
invoking a defined method on an identified object would apply
semantics.  The method token is <b><u>case-sensitive</u></b> because it might be
used as a gateway to object-based systems with <b><u>case-sensitive</u></b> method
names.

.
.
.

This specification defines a number of standardized methods that are
commonly used in HTTP, as outlined by the following table.  By
convention, standardized methods are defined in <b><u>all-uppercase</u></b>
US-ASCII letters.

+---------+-------------------------------------------------+-------+
| Method  | Description                                     | Sec.  |
+---------+-------------------------------------------------+-------+
| GET     | Transfer a current representation of the target | 4.3.1 |
|         | resource.                                       |       |
| HEAD    | Same as GET, but only transfer the status line  | 4.3.2 |
|         | and header section.                             |       |
| POST    | Perform resource-specific processing on the     | 4.3.3 |
|         | request payload.                                |       |
| PUT     | Replace all current representations of the      | 4.3.4 |
|         | target resource with the request payload.       |       |
| DELETE  | Remove all current representations of the       | 4.3.5 |
|         | target resource.                                |       |
| CONNECT | Establish a tunnel to the server identified by  | 4.3.6 |
|         | the target resource.                            |       |
| OPTIONS | Describe the communication options for the      | 4.3.7 |
|         | target resource.                                |       |
| TRACE   | Perform a message loop-back test along the path | 4.3.8 |
|         | to the target resource.                         |       |
+---------+-------------------------------------------------+-------+

All general-purpose servers MUST support the methods GET and HEAD.
All other methods are OPTIONAL.
    </code>
  </pre>
</div>

:)
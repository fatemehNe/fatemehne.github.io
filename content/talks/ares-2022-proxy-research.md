+++
title = "ARES 2022: Web proxy misbehavior analysis"
date = "2022-08-23"
description = "Published research on HTTP/S proxy behavior at ARES 2022, using a threat intelligence lens to predict manipulations before they happen. The work was presented and defended as master's thesis."
location = "Vienna, Austria"
tags = ["Research", "Talk", "Threat Intelligence"]
aliases = ["ares-2022", "web-proxy-misbehavior"]
author = "Fatima Nezhadian"
+++

The need for anonymity and privacy has increased use of open web proxies that relay traffic between clients and servers. Our ARES 2022 work examined how these proxies alter content and whether we can **predict manipulations before fetching through the proxy**.

Abstract highlights:
- New approach to proactively predict the types of content alterations silently injected by open proxies, without first fetching through them.
- Evaluated on 1,028 domains via 1,293 proxies; derived 13 manipulation types and predicted them with ~90% accuracy.
- Found most proxies alter content based on technical traits of the website and server.

Session coverage:
- Detection and analysis of HTTP/S proxy tampering.
- Feature signals that indicate likely manipulations and how to use them for proactive defense.
- Why prediction matters for threat intelligence and safer browsing through open proxies.

Read the paper: [ACM Digital Library](https://dl.acm.org/doi/abs/10.1145/3538969.3544412)

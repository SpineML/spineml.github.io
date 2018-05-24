---
layout: default
title: Simulators
---

A simulation tool-chain for the SpineML format is available using XSLT code generation for three simulators; [BRAHMS], [DAMSON] and [PyNN]. Each simulator requires a complete set of (schema validated) component, network and experiment descriptions of a model. A set of simulator specific templates is then applied to the model to automatically generate simulation code compatible with simulator. Logging and outputs will generated in the native simulator format.

![XSLT code generation tool-chain for the SpineML format](/public/images/Toolchain_v1.png){: .center-image }

  [BRAHMS]: /simulators/BRAHMS/ "wikilink"
  [DAMSON]: /simulators/damson/ "wikilink"
  [PyNN]: /simulators/PyNN/ "wikilink"

---
layout: default
title: Publications
---

To reference SpineCreator or the code generation tool-chain, please
use:

**Cope, A.J., Richmond, P., James, S.S. et al.** “SpineCreator: a
  Graphical User Interface for the Creation of Layered Neural Models”
  [*Neuroinformatics (2016). doi:10.1007/s12021-016-9311-z*](http://link.springer.com/article/10.1007/s12021-016-9311-z)

To reference the SpineML language please use this publication:

**Richmond P, Cope A, Gurney K, Allerton DJ.** “From Model Specification to Simulation of Biologically Constrained Networks of Spiking Neurons” *Neuroinformatics. 2013 Nov 20*

The following poster is available describing an overview of the toolchain.

**Cope AJ, Richmond P.** “A toolchain for creation of spiking neural networks utilising NineML - a tool independent XML model description format” *Poster presentation at eFutures workshop \`Building Bridges to Building Brains\` Edinburgh 2012* [available here](/public/images/e-futures_building_bridges_to_building_brains.pdf)

The following publications use SpineML and SpineCreator, if you would like to add a paper to this list please contact us:

<ul class="list-of-papers">
	{% for paper in site.data.papers %}
	<li>
		<h3>"{{paper.title}}"</h3>
		<h4>{{paper.author-list}}</h4>
		<p class="publication">{{paper.publication}}</p>
		<p class="keywords">
			<span>Keywords:</span>
			{{paper.keywords}}
		</p>
		<p class="description">{{paper.description}}</p>
		{% if paper.url || paper.bib %}
			{% if paper.url %}
				<a class="paper-button" href="{{paper.url}}">link</a>
			{% endif %}
			{% if paper.bib %}
				<a class="paper-button" href="{{paper.bib}}">cite</a>
			{% endif %}
		{% endif %}
	</li>
	{% endfor %}
</ul>

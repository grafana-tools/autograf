# Autograf is a dashboard constructor for Grafana [![Go Report Card](https://goreportcard.com/badge/github.com/grafov/autograf)](https://goreportcard.com/report/github.com/grafov/autograf)

*Moving from github.com/grafov/autograf repository in progress. Some things may not work.*
*The libraries moved out to github.com/grafana-tools/sdk.*

[Grafana](http://grafana.org) is flexible and usable for exploring and visualizing data. But UI of Grafana is not very suitable for repetitive operations with large number of objects on multiple dashboards. Aim of Autograf project is help with maintaining a large set of dashboards and datasources in an automated way. Autograf will not try to be a replacement for native Grafana methods of automation (templating variables, repeatable panels and scripted dashboards) but it complement them with own way. But I think DSL with a plain blocks of text without complex nesting will good for representing Grafana board-row-panel concept.

This project is in early stage of development and does nothing useful now. Firstly it offers a way for processing dashboards in Go apps and interacting with Grafana instances. This part of project is already usable but it was moved to a separate repository called [sdk](https://github.com/grafana-tools/sdk). Secondly it will offer DSL for constructing dashboards. DSL part is unfinished yet so it will be published later.

## Thoughts about DSL

Work on DSL and translator in progress so not much to say about it yet. I want something with syntax short for writing
and clean for reading for describing dashboards in a style of programming language. Instead of mapping them 1:1 to JSON 
syntax of Grafana objects. The short sample how it may look:

    # Example of a board with a panel
    board The sample dashboard

	# Example how define new sources in the loop:
	repeat (0..3) as $srv:
      source PromSrc$srv
      prometheus http://127.1:9090

	# This example defines the set of panels with different sources.
	var n = 0
	for $handlers as handler:
	  $n++
	  panel Sample number $n
	  type graph
	  query my_response_time_metric{instance="host$n",handler="$handler"}
	  if $n < 4:
	    datasource PromSrc$n
	  else:
		datasource Prom0

## Roadmap

* `[PROGRESS]` Realize DSL for defining dashboards in a plain text format.
* Import dashboards or single panels from running Grafana instances and convert them to DSL.

### Projects offered DSL or helper tools for Grafana in other languages

* [github.com/jakubplichta/grafana-dashboard-builder](https://github.com/jakubplichta/grafana-dashboard-builder) Python tool for building Grafana dashboards in YAML.
* [github.com/m110/grafcli](https://github.com/m110/grafcli) Python tool for managing Grafana in CLI. It querying Grafana backends directly. The project abandoned in alpha state for a long time.

cola Report for Consensus Partitioning
==================

**Date**: 2019-12-03 13:43:01 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21167    52
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:kmeans](#SD-kmeans)     |          3| 1.000|           0.981|       0.986|** |           |
|[SD:skmeans](#SD-skmeans)   |          3| 1.000|           0.982|       0.993|** |           |
|[SD:pam](#SD-pam)           |          3| 1.000|           0.958|       0.985|** |2          |
|[SD:mclust](#SD-mclust)     |          2| 1.000|           0.981|       0.987|** |           |
|[CV:kmeans](#CV-kmeans)     |          3| 1.000|           0.975|       0.981|** |           |
|[CV:skmeans](#CV-skmeans)   |          3| 1.000|           0.969|       0.989|** |           |
|[CV:mclust](#CV-mclust)     |          2| 1.000|           0.976|       0.986|** |           |
|[CV:NMF](#CV-NMF)           |          3| 1.000|           0.973|       0.988|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 1.000|           0.969|       0.978|** |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 1.000|           0.960|       0.985|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           0.998|       0.995|** |           |
|[ATC:skmeans](#ATC-skmeans) |          3| 1.000|           0.993|       0.996|** |2          |
|[CV:pam](#CV-pam)           |          3| 0.999|           0.962|       0.985|** |2          |
|[SD:NMF](#SD-NMF)           |          3| 0.998|           0.948|       0.978|** |           |
|[MAD:mclust](#MAD-mclust)   |          3| 0.979|           0.962|       0.977|** |2          |
|[MAD:NMF](#MAD-NMF)         |          3| 0.969|           0.917|       0.968|** |           |
|[MAD:pam](#MAD-pam)         |          3| 0.967|           0.927|       0.973|** |           |
|[ATC:NMF](#ATC-NMF)         |          3| 0.922|           0.925|       0.968|*  |2          |
|[ATC:pam](#ATC-pam)         |          3| 0.772|           0.924|       0.967|   |           |
|[ATC:mclust](#ATC-mclust)   |          4| 0.765|           0.789|       0.900|   |           |
|[ATC:hclust](#ATC-hclust)   |          3| 0.684|           0.818|       0.904|   |           |
|[CV:hclust](#CV-hclust)     |          3| 0.662|           0.830|       0.897|   |           |
|[MAD:hclust](#MAD-hclust)   |          3| 0.385|           0.617|       0.801|   |           |
|[SD:hclust](#SD-hclust)     |          2| 0.381|           0.750|       0.848|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.422           0.782       0.858          0.452 0.527   0.527
#&gt; CV:NMF      2 0.418           0.820       0.861          0.450 0.527   0.527
#&gt; MAD:NMF     2 0.880           0.944       0.973          0.479 0.527   0.527
#&gt; ATC:NMF     2 1.000           0.958       0.982          0.405 0.599   0.599
#&gt; SD:skmeans  2 0.571           0.918       0.933          0.481 0.517   0.517
#&gt; CV:skmeans  2 0.524           0.899       0.912          0.476 0.517   0.517
#&gt; MAD:skmeans 2 0.847           0.945       0.976          0.486 0.517   0.517
#&gt; ATC:skmeans 2 1.000           0.969       0.987          0.500 0.497   0.497
#&gt; SD:mclust   2 1.000           0.981       0.987          0.328 0.683   0.683
#&gt; CV:mclust   2 1.000           0.976       0.986          0.331 0.683   0.683
#&gt; MAD:mclust  2 1.000           0.978       0.974          0.324 0.683   0.683
#&gt; ATC:mclust  2 0.451           0.781       0.770          0.369 0.517   0.517
#&gt; SD:kmeans   2 0.482           0.606       0.774          0.375 0.502   0.502
#&gt; CV:kmeans   2 0.497           0.756       0.795          0.376 0.581   0.581
#&gt; MAD:kmeans  2 0.491           0.786       0.850          0.414 0.538   0.538
#&gt; ATC:kmeans  2 0.479           0.832       0.843          0.419 0.566   0.566
#&gt; SD:pam      2 1.000           0.966       0.973          0.327 0.683   0.683
#&gt; CV:pam      2 1.000           0.975       0.974          0.320 0.683   0.683
#&gt; MAD:pam     2 0.473           0.747       0.840          0.372 0.599   0.599
#&gt; ATC:pam     2 0.501           0.769       0.861          0.329 0.735   0.735
#&gt; SD:hclust   2 0.381           0.750       0.848          0.397 0.527   0.527
#&gt; CV:hclust   2 0.409           0.807       0.871          0.375 0.618   0.618
#&gt; MAD:hclust  2 0.685           0.833       0.931          0.364 0.638   0.638
#&gt; ATC:hclust  2 0.788           0.961       0.973          0.417 0.566   0.566
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.998           0.948       0.978          0.376 0.687   0.490
#&gt; CV:NMF      3 1.000           0.973       0.988          0.367 0.702   0.509
#&gt; MAD:NMF     3 0.969           0.917       0.968          0.342 0.775   0.593
#&gt; ATC:NMF     3 0.922           0.925       0.968          0.658 0.660   0.463
#&gt; SD:skmeans  3 1.000           0.982       0.993          0.344 0.773   0.586
#&gt; CV:skmeans  3 1.000           0.969       0.989          0.355 0.773   0.586
#&gt; MAD:skmeans 3 1.000           0.960       0.985          0.339 0.773   0.586
#&gt; ATC:skmeans 3 1.000           0.993       0.996          0.357 0.738   0.515
#&gt; SD:mclust   3 0.687           0.819       0.911          0.896 0.704   0.567
#&gt; CV:mclust   3 0.628           0.842       0.908          0.855 0.716   0.584
#&gt; MAD:mclust  3 0.979           0.962       0.977          0.903 0.704   0.567
#&gt; ATC:mclust  3 0.509           0.822       0.850          0.704 0.756   0.556
#&gt; SD:kmeans   3 1.000           0.981       0.986          0.620 0.849   0.710
#&gt; CV:kmeans   3 1.000           0.975       0.981          0.618 0.793   0.647
#&gt; MAD:kmeans  3 1.000           0.969       0.978          0.472 0.740   0.563
#&gt; ATC:kmeans  3 1.000           0.998       0.995          0.569 0.775   0.601
#&gt; SD:pam      3 1.000           0.958       0.985          0.786 0.729   0.603
#&gt; CV:pam      3 0.999           0.962       0.985          0.783 0.743   0.624
#&gt; MAD:pam     3 0.967           0.927       0.973          0.630 0.633   0.463
#&gt; ATC:pam     3 0.772           0.924       0.967          0.756 0.667   0.551
#&gt; SD:hclust   3 0.642           0.695       0.855          0.392 0.621   0.431
#&gt; CV:hclust   3 0.662           0.830       0.897          0.448 0.781   0.646
#&gt; MAD:hclust  3 0.385           0.617       0.801          0.530 0.743   0.603
#&gt; ATC:hclust  3 0.684           0.818       0.904          0.572 0.759   0.573
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.730           0.769       0.885         0.1802 0.855   0.638
#&gt; CV:NMF      4 0.707           0.703       0.854         0.1689 0.867   0.665
#&gt; MAD:NMF     4 0.680           0.625       0.834         0.1157 0.825   0.570
#&gt; ATC:NMF     4 0.619           0.606       0.789         0.0956 0.900   0.715
#&gt; SD:skmeans  4 0.866           0.932       0.954         0.1661 0.851   0.600
#&gt; CV:skmeans  4 0.847           0.907       0.931         0.1694 0.851   0.600
#&gt; MAD:skmeans 4 0.844           0.852       0.908         0.1514 0.851   0.600
#&gt; ATC:skmeans 4 0.898           0.901       0.926         0.0867 0.926   0.776
#&gt; SD:mclust   4 0.703           0.562       0.820         0.1283 0.873   0.695
#&gt; CV:mclust   4 0.699           0.548       0.805         0.1460 0.925   0.811
#&gt; MAD:mclust  4 0.770           0.819       0.871         0.1028 0.988   0.969
#&gt; ATC:mclust  4 0.765           0.789       0.900         0.1436 0.888   0.682
#&gt; SD:kmeans   4 0.704           0.629       0.814         0.1622 0.931   0.824
#&gt; CV:kmeans   4 0.677           0.706       0.824         0.1463 0.931   0.825
#&gt; MAD:kmeans  4 0.681           0.616       0.814         0.1623 0.876   0.696
#&gt; ATC:kmeans  4 0.694           0.741       0.776         0.1151 0.889   0.680
#&gt; SD:pam      4 0.855           0.932       0.947         0.2300 0.853   0.652
#&gt; CV:pam      4 0.832           0.895       0.944         0.2335 0.860   0.677
#&gt; MAD:pam     4 0.816           0.878       0.934         0.2014 0.843   0.625
#&gt; ATC:pam     4 0.734           0.855       0.928         0.2663 0.791   0.524
#&gt; SD:hclust   4 0.514           0.586       0.773         0.1931 0.848   0.668
#&gt; CV:hclust   4 0.530           0.787       0.828         0.1909 0.957   0.895
#&gt; MAD:hclust  4 0.408           0.609       0.741         0.1979 0.928   0.830
#&gt; ATC:hclust  4 0.713           0.682       0.827         0.0640 0.989   0.965
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.700           0.638       0.793         0.0693 0.922   0.729
#&gt; CV:NMF      5 0.702           0.637       0.817         0.0822 0.845   0.524
#&gt; MAD:NMF     5 0.654           0.671       0.795         0.0905 0.839   0.516
#&gt; ATC:NMF     5 0.672           0.699       0.828         0.0743 0.881   0.595
#&gt; SD:skmeans  5 0.804           0.698       0.839         0.0574 0.907   0.658
#&gt; CV:skmeans  5 0.771           0.690       0.843         0.0574 0.913   0.674
#&gt; MAD:skmeans 5 0.801           0.688       0.830         0.0582 0.857   0.526
#&gt; ATC:skmeans 5 0.857           0.894       0.909         0.0712 0.942   0.781
#&gt; SD:mclust   5 0.707           0.772       0.808         0.0624 0.910   0.715
#&gt; CV:mclust   5 0.683           0.670       0.801         0.0472 0.851   0.607
#&gt; MAD:mclust  5 0.736           0.771       0.802         0.0905 0.834   0.572
#&gt; ATC:mclust  5 0.697           0.788       0.856         0.0478 0.920   0.709
#&gt; SD:kmeans   5 0.657           0.589       0.722         0.0834 0.923   0.776
#&gt; CV:kmeans   5 0.659           0.592       0.743         0.0882 0.923   0.777
#&gt; MAD:kmeans  5 0.665           0.640       0.770         0.0882 0.910   0.720
#&gt; ATC:kmeans  5 0.770           0.675       0.784         0.0658 0.804   0.413
#&gt; SD:pam      5 0.823           0.902       0.929         0.0234 0.988   0.958
#&gt; CV:pam      5 0.825           0.886       0.934         0.0233 0.988   0.959
#&gt; MAD:pam     5 0.829           0.881       0.931         0.0240 0.986   0.948
#&gt; ATC:pam     5 0.725           0.614       0.794         0.0791 0.864   0.551
#&gt; SD:hclust   5 0.645           0.677       0.800         0.0745 0.950   0.868
#&gt; CV:hclust   5 0.658           0.654       0.807         0.1100 0.969   0.918
#&gt; MAD:hclust  5 0.577           0.615       0.756         0.1158 0.836   0.597
#&gt; ATC:hclust  5 0.736           0.660       0.781         0.0917 0.891   0.671
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.716           0.631       0.786         0.0509 0.847   0.451
#&gt; CV:NMF      6 0.740           0.657       0.799         0.0565 0.870   0.514
#&gt; MAD:NMF     6 0.671           0.633       0.771         0.0556 0.916   0.654
#&gt; ATC:NMF     6 0.629           0.511       0.711         0.0380 0.956   0.801
#&gt; SD:skmeans  6 0.781           0.547       0.789         0.0331 0.965   0.844
#&gt; CV:skmeans  6 0.759           0.539       0.765         0.0349 0.956   0.797
#&gt; MAD:skmeans 6 0.772           0.549       0.783         0.0341 0.990   0.955
#&gt; ATC:skmeans 6 0.846           0.798       0.853         0.0391 0.973   0.869
#&gt; SD:mclust   6 0.715           0.707       0.818         0.0400 0.955   0.820
#&gt; CV:mclust   6 0.730           0.633       0.787         0.0484 0.948   0.830
#&gt; MAD:mclust  6 0.858           0.864       0.921         0.0715 0.952   0.800
#&gt; ATC:mclust  6 0.749           0.710       0.813         0.0653 0.925   0.685
#&gt; SD:kmeans   6 0.669           0.577       0.705         0.0493 0.848   0.513
#&gt; CV:kmeans   6 0.674           0.544       0.702         0.0667 0.836   0.485
#&gt; MAD:kmeans  6 0.694           0.578       0.737         0.0555 0.851   0.504
#&gt; ATC:kmeans  6 0.764           0.789       0.839         0.0379 0.941   0.751
#&gt; SD:pam      6 0.758           0.794       0.871         0.0325 0.981   0.932
#&gt; CV:pam      6 0.850           0.830       0.917         0.0230 0.993   0.976
#&gt; MAD:pam     6 0.719           0.629       0.830         0.0523 0.946   0.803
#&gt; ATC:pam     6 0.782           0.781       0.888         0.0447 0.834   0.396
#&gt; SD:hclust   6 0.651           0.642       0.749         0.0507 0.905   0.744
#&gt; CV:hclust   6 0.643           0.445       0.776         0.0479 0.947   0.852
#&gt; MAD:hclust  6 0.642           0.567       0.755         0.0481 0.876   0.564
#&gt; ATC:hclust  6 0.773           0.710       0.855         0.0338 0.958   0.826
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      49         1.10e-01 2
#&gt; CV:NMF      50         1.22e-01 2
#&gt; MAD:NMF     52         2.84e-01 2
#&gt; ATC:NMF     52         7.24e-01 2
#&gt; SD:skmeans  52         6.10e-01 2
#&gt; CV:skmeans  52         6.10e-01 2
#&gt; MAD:skmeans 52         6.10e-01 2
#&gt; ATC:skmeans 51         3.92e-01 2
#&gt; SD:mclust   52         1.99e-10 2
#&gt; CV:mclust   52         1.99e-10 2
#&gt; MAD:mclust  52         1.99e-10 2
#&gt; ATC:mclust  45         1.00e+00 2
#&gt; SD:kmeans   28               NA 2
#&gt; CV:kmeans   42         7.20e-01 2
#&gt; MAD:kmeans  49         5.19e-01 2
#&gt; ATC:kmeans  52         5.15e-01 2
#&gt; SD:pam      52         1.99e-10 2
#&gt; CV:pam      52         1.99e-10 2
#&gt; MAD:pam     48         1.09e-09 2
#&gt; ATC:pam     50         1.00e+00 2
#&gt; SD:hclust   47         1.81e-01 2
#&gt; CV:hclust   50         2.90e-01 2
#&gt; MAD:hclust  48         5.03e-01 2
#&gt; ATC:hclust  52         5.15e-01 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      51         1.31e-10 3
#&gt; CV:NMF      52         8.27e-11 3
#&gt; MAD:NMF     49         4.55e-09 3
#&gt; ATC:NMF     51         3.28e-02 3
#&gt; SD:skmeans  52         1.31e-09 3
#&gt; CV:skmeans  51         1.98e-09 3
#&gt; MAD:skmeans 51         2.00e-09 3
#&gt; ATC:skmeans 52         4.76e-02 3
#&gt; SD:mclust   49         3.24e-10 3
#&gt; CV:mclust   49         3.24e-10 3
#&gt; MAD:mclust  52         8.27e-11 3
#&gt; ATC:mclust  51         2.34e-02 3
#&gt; SD:kmeans   52         8.27e-11 3
#&gt; CV:kmeans   52         8.27e-11 3
#&gt; MAD:kmeans  52         9.23e-10 3
#&gt; ATC:kmeans  52         3.39e-01 3
#&gt; SD:pam      51         1.24e-10 3
#&gt; CV:pam      52         7.80e-11 3
#&gt; MAD:pam     50         2.49e-10 3
#&gt; ATC:pam     51         8.17e-01 3
#&gt; SD:hclust   41         2.26e-08 3
#&gt; CV:hclust   50         2.05e-10 3
#&gt; MAD:hclust  38         8.40e-08 3
#&gt; ATC:hclust  50         1.16e-02 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      44         3.12e-08 4
#&gt; CV:NMF      45         1.58e-08 4
#&gt; MAD:NMF     36         7.39e-07 4
#&gt; ATC:NMF     41         8.42e-04 4
#&gt; SD:skmeans  52         6.69e-09 4
#&gt; CV:skmeans  51         1.01e-08 4
#&gt; MAD:skmeans 50         2.17e-08 4
#&gt; ATC:skmeans 52         3.19e-02 4
#&gt; SD:mclust   27         9.27e-06 4
#&gt; CV:mclust   38         5.20e-08 4
#&gt; MAD:mclust  50         1.37e-09 4
#&gt; ATC:mclust  47         7.96e-05 4
#&gt; SD:kmeans   36         1.30e-07 4
#&gt; CV:kmeans   45         1.93e-09 4
#&gt; MAD:kmeans  39         3.60e-08 4
#&gt; ATC:kmeans  49         2.18e-02 4
#&gt; SD:pam      51         6.63e-10 4
#&gt; CV:pam      51         6.63e-10 4
#&gt; MAD:pam     50         1.26e-09 4
#&gt; ATC:pam     51         1.49e-02 4
#&gt; SD:hclust   46         6.53e-09 4
#&gt; CV:hclust   50         1.11e-09 4
#&gt; MAD:hclust  40         9.62e-08 4
#&gt; ATC:hclust  49         1.36e-02 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      44         7.87e-08 5
#&gt; CV:NMF      41         1.10e-07 5
#&gt; MAD:NMF     46         2.74e-07 5
#&gt; ATC:NMF     47         5.90e-05 5
#&gt; SD:skmeans  40         3.95e-07 5
#&gt; CV:skmeans  42         1.66e-07 5
#&gt; MAD:skmeans 38         2.79e-06 5
#&gt; ATC:skmeans 52         4.66e-02 5
#&gt; SD:mclust   50         4.77e-09 5
#&gt; CV:mclust   45         1.55e-08 5
#&gt; MAD:mclust  48         1.40e-08 5
#&gt; ATC:mclust  48         2.46e-05 5
#&gt; SD:kmeans   34         3.24e-07 5
#&gt; CV:kmeans   40         1.97e-08 5
#&gt; MAD:kmeans  39         5.73e-07 5
#&gt; ATC:kmeans  39         3.49e-03 5
#&gt; SD:pam      51         2.87e-09 5
#&gt; CV:pam      51         2.87e-09 5
#&gt; MAD:pam     51         2.87e-09 5
#&gt; ATC:pam     34         9.04e-04 5
#&gt; SD:hclust   43         2.96e-08 5
#&gt; CV:hclust   38         8.67e-07 5
#&gt; MAD:hclust  35         3.15e-06 5
#&gt; ATC:hclust  48         1.63e-02 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      41         9.38e-08 6
#&gt; CV:NMF      42         5.89e-08 6
#&gt; MAD:NMF     38         9.49e-07 6
#&gt; ATC:NMF     32         1.64e-04 6
#&gt; SD:skmeans  32         3.80e-06 6
#&gt; CV:skmeans  33         8.40e-06 6
#&gt; MAD:skmeans 31         1.82e-04 6
#&gt; ATC:skmeans 49         6.23e-02 6
#&gt; SD:mclust   45         1.75e-07 6
#&gt; CV:mclust   38         3.39e-07 6
#&gt; MAD:mclust  51         1.14e-08 6
#&gt; ATC:mclust  45         2.69e-07 6
#&gt; SD:kmeans   36         6.48e-06 6
#&gt; CV:kmeans   34         1.26e-05 6
#&gt; MAD:kmeans  29         4.63e-05 6
#&gt; ATC:kmeans  49         1.47e-02 6
#&gt; SD:pam      46         2.54e-08 6
#&gt; CV:pam      49         6.75e-09 6
#&gt; MAD:pam     38         8.67e-07 6
#&gt; ATC:pam     49         4.81e-04 6
#&gt; SD:hclust   44         2.32e-08 6
#&gt; CV:hclust   25         8.49e-05 6
#&gt; MAD:hclust  32         3.76e-05 6
#&gt; ATC:hclust  47         4.08e-02 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.381           0.750       0.848         0.3974 0.527   0.527
#> 3 3 0.642           0.695       0.855         0.3922 0.621   0.431
#> 4 4 0.514           0.586       0.773         0.1931 0.848   0.668
#> 5 5 0.645           0.677       0.800         0.0745 0.950   0.868
#> 6 6 0.651           0.642       0.749         0.0507 0.905   0.744
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.000     0.7741 1.000 0.000
#&gt; GSM559387     1   0.000     0.7741 1.000 0.000
#&gt; GSM559391     1   0.000     0.7741 1.000 0.000
#&gt; GSM559395     1   0.000     0.7741 1.000 0.000
#&gt; GSM559397     1   0.000     0.7741 1.000 0.000
#&gt; GSM559401     1   0.000     0.7741 1.000 0.000
#&gt; GSM559414     1   0.000     0.7741 1.000 0.000
#&gt; GSM559422     1   0.118     0.7646 0.984 0.016
#&gt; GSM559424     1   0.000     0.7741 1.000 0.000
#&gt; GSM559431     2   0.000     0.7794 0.000 1.000
#&gt; GSM559432     1   0.118     0.7646 0.984 0.016
#&gt; GSM559381     1   0.850     0.7711 0.724 0.276
#&gt; GSM559382     2   0.891     0.5178 0.308 0.692
#&gt; GSM559384     1   0.714     0.8893 0.804 0.196
#&gt; GSM559385     1   0.714     0.8893 0.804 0.196
#&gt; GSM559386     2   0.932     0.4443 0.348 0.652
#&gt; GSM559388     2   0.861     0.5551 0.284 0.716
#&gt; GSM559389     1   0.788     0.8385 0.764 0.236
#&gt; GSM559390     1   0.767     0.8569 0.776 0.224
#&gt; GSM559392     2   0.000     0.7794 0.000 1.000
#&gt; GSM559393     1   0.714     0.8893 0.804 0.196
#&gt; GSM559394     1   0.714     0.8893 0.804 0.196
#&gt; GSM559396     1   0.714     0.8893 0.804 0.196
#&gt; GSM559398     2   0.000     0.7794 0.000 1.000
#&gt; GSM559399     1   0.714     0.8893 0.804 0.196
#&gt; GSM559400     2   0.662     0.6778 0.172 0.828
#&gt; GSM559402     1   0.714     0.8893 0.804 0.196
#&gt; GSM559403     1   0.714     0.8893 0.804 0.196
#&gt; GSM559404     1   0.714     0.8893 0.804 0.196
#&gt; GSM559405     1   0.730     0.8806 0.796 0.204
#&gt; GSM559406     1   0.714     0.8893 0.804 0.196
#&gt; GSM559407     1   0.714     0.8893 0.804 0.196
#&gt; GSM559408     1   0.714     0.8893 0.804 0.196
#&gt; GSM559409     1   0.714     0.8893 0.804 0.196
#&gt; GSM559410     1   0.714     0.8893 0.804 0.196
#&gt; GSM559411     1   0.714     0.8893 0.804 0.196
#&gt; GSM559412     1   0.714     0.8893 0.804 0.196
#&gt; GSM559413     1   0.714     0.8893 0.804 0.196
#&gt; GSM559415     2   1.000    -0.0703 0.500 0.500
#&gt; GSM559416     2   0.988     0.2114 0.436 0.564
#&gt; GSM559417     2   0.988     0.2114 0.436 0.564
#&gt; GSM559418     2   1.000    -0.0703 0.500 0.500
#&gt; GSM559419     1   0.714     0.8893 0.804 0.196
#&gt; GSM559420     1   0.714     0.8893 0.804 0.196
#&gt; GSM559421     2   0.000     0.7794 0.000 1.000
#&gt; GSM559423     2   0.000     0.7794 0.000 1.000
#&gt; GSM559425     2   0.000     0.7794 0.000 1.000
#&gt; GSM559426     2   0.118     0.7775 0.016 0.984
#&gt; GSM559427     2   0.000     0.7794 0.000 1.000
#&gt; GSM559428     2   0.118     0.7775 0.016 0.984
#&gt; GSM559429     2   0.118     0.7775 0.016 0.984
#&gt; GSM559430     2   0.000     0.7794 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559387     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559391     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559395     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559397     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559401     3   0.650     0.4915 0.004 0.460 0.536
#&gt; GSM559414     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559422     3   0.652     0.4736 0.004 0.480 0.516
#&gt; GSM559424     3   0.000     0.7043 0.000 0.000 1.000
#&gt; GSM559431     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559432     3   0.652     0.4736 0.004 0.480 0.516
#&gt; GSM559381     1   0.740     0.6944 0.552 0.036 0.412
#&gt; GSM559382     1   0.968    -0.0715 0.448 0.320 0.232
#&gt; GSM559384     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559385     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559386     1   0.978     0.1042 0.436 0.304 0.260
#&gt; GSM559388     1   0.950    -0.1486 0.476 0.316 0.208
#&gt; GSM559389     1   0.748     0.7296 0.512 0.036 0.452
#&gt; GSM559390     1   0.628     0.7541 0.540 0.000 0.460
#&gt; GSM559392     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559393     1   0.652     0.7736 0.516 0.004 0.480
#&gt; GSM559394     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559396     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559398     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559399     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559400     1   0.618    -0.4865 0.732 0.236 0.032
#&gt; GSM559402     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559403     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559404     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559405     1   0.630     0.7716 0.528 0.000 0.472
#&gt; GSM559406     1   0.631     0.7679 0.508 0.000 0.492
#&gt; GSM559407     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559408     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559409     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559410     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559411     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559412     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559413     1   0.630     0.7744 0.516 0.000 0.484
#&gt; GSM559415     1   0.781     0.4986 0.640 0.092 0.268
#&gt; GSM559416     1   0.872     0.4239 0.576 0.152 0.272
#&gt; GSM559417     1   0.872     0.4239 0.576 0.152 0.272
#&gt; GSM559418     1   0.781     0.4986 0.640 0.092 0.268
#&gt; GSM559419     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559420     1   0.630     0.7749 0.520 0.000 0.480
#&gt; GSM559421     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559423     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559425     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559426     2   0.682     0.9798 0.472 0.516 0.012
#&gt; GSM559427     2   0.630     0.9917 0.480 0.520 0.000
#&gt; GSM559428     2   0.682     0.9668 0.484 0.504 0.012
#&gt; GSM559429     2   0.682     0.9798 0.472 0.516 0.012
#&gt; GSM559430     2   0.630     0.9917 0.480 0.520 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559387     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559391     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559395     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559397     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559401     3  0.4086      0.563 0.008 0.000 0.776 0.216
#&gt; GSM559414     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559422     3  0.3975      0.546 0.000 0.000 0.760 0.240
#&gt; GSM559424     3  0.4164      0.809 0.264 0.000 0.736 0.000
#&gt; GSM559431     2  0.3311      0.620 0.000 0.828 0.000 0.172
#&gt; GSM559432     3  0.3975      0.546 0.000 0.000 0.760 0.240
#&gt; GSM559381     1  0.4361      0.548 0.772 0.020 0.000 0.208
#&gt; GSM559382     2  0.7863     -0.482 0.276 0.380 0.000 0.344
#&gt; GSM559384     1  0.1297      0.768 0.964 0.000 0.016 0.020
#&gt; GSM559385     1  0.3166      0.743 0.868 0.000 0.016 0.116
#&gt; GSM559386     2  0.7924     -0.591 0.328 0.340 0.000 0.332
#&gt; GSM559388     2  0.7812     -0.421 0.264 0.408 0.000 0.328
#&gt; GSM559389     1  0.3900      0.647 0.816 0.020 0.000 0.164
#&gt; GSM559390     1  0.3726      0.572 0.788 0.000 0.000 0.212
#&gt; GSM559392     2  0.2408      0.689 0.000 0.896 0.000 0.104
#&gt; GSM559393     1  0.3408      0.738 0.860 0.004 0.016 0.120
#&gt; GSM559394     1  0.3166      0.743 0.868 0.000 0.016 0.116
#&gt; GSM559396     1  0.1297      0.768 0.964 0.000 0.016 0.020
#&gt; GSM559398     2  0.2408      0.689 0.000 0.896 0.000 0.104
#&gt; GSM559399     1  0.2216      0.754 0.908 0.000 0.000 0.092
#&gt; GSM559400     2  0.6200      0.173 0.052 0.504 0.000 0.444
#&gt; GSM559402     1  0.0657      0.770 0.984 0.000 0.012 0.004
#&gt; GSM559403     1  0.2281      0.753 0.904 0.000 0.000 0.096
#&gt; GSM559404     1  0.5179      0.514 0.728 0.000 0.220 0.052
#&gt; GSM559405     1  0.2281      0.751 0.904 0.000 0.000 0.096
#&gt; GSM559406     1  0.4163      0.619 0.792 0.000 0.020 0.188
#&gt; GSM559407     1  0.0657      0.770 0.984 0.000 0.012 0.004
#&gt; GSM559408     1  0.1042      0.767 0.972 0.000 0.020 0.008
#&gt; GSM559409     1  0.1042      0.767 0.972 0.000 0.020 0.008
#&gt; GSM559410     1  0.2546      0.757 0.900 0.000 0.008 0.092
#&gt; GSM559411     1  0.1059      0.767 0.972 0.000 0.012 0.016
#&gt; GSM559412     1  0.4562      0.545 0.764 0.000 0.208 0.028
#&gt; GSM559413     1  0.4562      0.545 0.764 0.000 0.208 0.028
#&gt; GSM559415     1  0.6987     -0.447 0.568 0.160 0.000 0.272
#&gt; GSM559416     4  0.7344      0.958 0.380 0.160 0.000 0.460
#&gt; GSM559417     4  0.7301      0.959 0.356 0.160 0.000 0.484
#&gt; GSM559418     1  0.6987     -0.447 0.568 0.160 0.000 0.272
#&gt; GSM559419     1  0.0469      0.771 0.988 0.000 0.000 0.012
#&gt; GSM559420     1  0.0469      0.771 0.988 0.000 0.000 0.012
#&gt; GSM559421     2  0.2216      0.691 0.000 0.908 0.000 0.092
#&gt; GSM559423     2  0.2281      0.691 0.000 0.904 0.000 0.096
#&gt; GSM559425     2  0.3311      0.620 0.000 0.828 0.000 0.172
#&gt; GSM559426     2  0.2473      0.670 0.012 0.908 0.000 0.080
#&gt; GSM559427     2  0.3311      0.620 0.000 0.828 0.000 0.172
#&gt; GSM559428     2  0.2796      0.663 0.016 0.892 0.000 0.092
#&gt; GSM559429     2  0.2542      0.669 0.012 0.904 0.000 0.084
#&gt; GSM559430     2  0.2149      0.692 0.000 0.912 0.000 0.088
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM559383     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559387     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559391     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559395     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559397     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559401     3  0.4307    -0.1779 0.000 0.000 0.500 0.500 NA
#&gt; GSM559414     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559422     4  0.1851     1.0000 0.000 0.000 0.088 0.912 NA
#&gt; GSM559424     3  0.0000     0.9174 0.000 0.000 1.000 0.000 NA
#&gt; GSM559431     2  0.3942     0.5637 0.000 0.728 0.000 0.012 NA
#&gt; GSM559432     4  0.1851     1.0000 0.000 0.000 0.088 0.912 NA
#&gt; GSM559381     1  0.3085     0.7344 0.852 0.032 0.000 0.000 NA
#&gt; GSM559382     2  0.6613     0.2946 0.352 0.468 0.000 0.008 NA
#&gt; GSM559384     1  0.3194     0.7958 0.832 0.000 0.020 0.000 NA
#&gt; GSM559385     1  0.1502     0.7966 0.940 0.000 0.000 0.004 NA
#&gt; GSM559386     2  0.6594     0.1771 0.404 0.428 0.000 0.008 NA
#&gt; GSM559388     2  0.6481     0.3382 0.340 0.496 0.000 0.008 NA
#&gt; GSM559389     1  0.2473     0.7645 0.896 0.032 0.000 0.000 NA
#&gt; GSM559390     1  0.3805     0.7498 0.784 0.000 0.000 0.032 NA
#&gt; GSM559392     2  0.0404     0.6979 0.000 0.988 0.000 0.000 NA
#&gt; GSM559393     1  0.1731     0.7949 0.932 0.004 0.000 0.004 NA
#&gt; GSM559394     1  0.1502     0.7966 0.940 0.000 0.000 0.004 NA
#&gt; GSM559396     1  0.3351     0.7948 0.828 0.000 0.020 0.004 NA
#&gt; GSM559398     2  0.0404     0.6979 0.000 0.988 0.000 0.000 NA
#&gt; GSM559399     1  0.0451     0.8015 0.988 0.000 0.000 0.004 NA
#&gt; GSM559400     2  0.6420     0.4171 0.100 0.596 0.000 0.048 NA
#&gt; GSM559402     1  0.2471     0.7980 0.864 0.000 0.000 0.000 NA
#&gt; GSM559403     1  0.0727     0.8016 0.980 0.000 0.004 0.004 NA
#&gt; GSM559404     1  0.5755     0.5550 0.604 0.000 0.060 0.024 NA
#&gt; GSM559405     1  0.0566     0.8014 0.984 0.000 0.000 0.004 NA
#&gt; GSM559406     1  0.3925     0.7557 0.784 0.000 0.004 0.032 NA
#&gt; GSM559407     1  0.2471     0.7980 0.864 0.000 0.000 0.000 NA
#&gt; GSM559408     1  0.2763     0.7949 0.848 0.000 0.004 0.000 NA
#&gt; GSM559409     1  0.2763     0.7949 0.848 0.000 0.004 0.000 NA
#&gt; GSM559410     1  0.0671     0.8028 0.980 0.000 0.000 0.004 NA
#&gt; GSM559411     1  0.2719     0.7966 0.852 0.000 0.004 0.000 NA
#&gt; GSM559412     1  0.5049     0.6225 0.644 0.000 0.060 0.000 NA
#&gt; GSM559413     1  0.5049     0.6225 0.644 0.000 0.060 0.000 NA
#&gt; GSM559415     1  0.5691     0.4065 0.632 0.236 0.000 0.004 NA
#&gt; GSM559416     1  0.7343     0.1016 0.408 0.236 0.000 0.032 NA
#&gt; GSM559417     1  0.7362     0.0554 0.392 0.236 0.000 0.032 NA
#&gt; GSM559418     1  0.5691     0.4065 0.632 0.236 0.000 0.004 NA
#&gt; GSM559419     1  0.2329     0.8020 0.876 0.000 0.000 0.000 NA
#&gt; GSM559420     1  0.2329     0.8020 0.876 0.000 0.000 0.000 NA
#&gt; GSM559421     2  0.0000     0.6991 0.000 1.000 0.000 0.000 NA
#&gt; GSM559423     2  0.0162     0.6995 0.000 0.996 0.000 0.000 NA
#&gt; GSM559425     2  0.3942     0.5637 0.000 0.728 0.000 0.012 NA
#&gt; GSM559426     2  0.3783     0.6641 0.012 0.768 0.000 0.004 NA
#&gt; GSM559427     2  0.3942     0.5637 0.000 0.728 0.000 0.012 NA
#&gt; GSM559428     2  0.4054     0.6565 0.012 0.744 0.000 0.008 NA
#&gt; GSM559429     2  0.3875     0.6596 0.012 0.756 0.000 0.004 NA
#&gt; GSM559430     2  0.0404     0.6986 0.000 0.988 0.000 0.000 NA
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3    p4  p5    p6
#&gt; GSM559383     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559387     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559391     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559395     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559397     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559401     3  0.3869   -0.00016 0.000 0.000 0.50 0.000 0.5 0.000
#&gt; GSM559414     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559422     5  0.0000    1.00000 0.000 0.000 0.00 0.000 1.0 0.000
#&gt; GSM559424     3  0.0000    0.92470 0.000 0.000 1.00 0.000 0.0 0.000
#&gt; GSM559431     2  0.0790    0.49872 0.000 0.968 0.00 0.000 0.0 0.032
#&gt; GSM559432     5  0.0000    1.00000 0.000 0.000 0.00 0.000 1.0 0.000
#&gt; GSM559381     1  0.4580    0.57283 0.708 0.004 0.00 0.120 0.0 0.168
#&gt; GSM559382     4  0.7415    0.59937 0.204 0.208 0.00 0.404 0.0 0.184
#&gt; GSM559384     1  0.2332    0.70666 0.904 0.000 0.02 0.036 0.0 0.040
#&gt; GSM559385     1  0.3834    0.64219 0.708 0.000 0.00 0.024 0.0 0.268
#&gt; GSM559386     4  0.7414    0.60832 0.252 0.168 0.00 0.392 0.0 0.188
#&gt; GSM559388     4  0.7443    0.56865 0.188 0.236 0.00 0.392 0.0 0.184
#&gt; GSM559389     1  0.4102    0.62655 0.752 0.004 0.00 0.080 0.0 0.164
#&gt; GSM559390     1  0.3404    0.58588 0.760 0.000 0.00 0.224 0.0 0.016
#&gt; GSM559392     2  0.3288    0.73816 0.000 0.724 0.00 0.276 0.0 0.000
#&gt; GSM559393     1  0.4045    0.63614 0.700 0.004 0.00 0.028 0.0 0.268
#&gt; GSM559394     1  0.3834    0.64219 0.708 0.000 0.00 0.024 0.0 0.268
#&gt; GSM559396     1  0.2401    0.70535 0.900 0.000 0.02 0.036 0.0 0.044
#&gt; GSM559398     2  0.3288    0.73816 0.000 0.724 0.00 0.276 0.0 0.000
#&gt; GSM559399     1  0.3240    0.66202 0.752 0.000 0.00 0.004 0.0 0.244
#&gt; GSM559400     4  0.4289   -0.17813 0.020 0.332 0.00 0.640 0.0 0.008
#&gt; GSM559402     1  0.0508    0.71641 0.984 0.000 0.00 0.012 0.0 0.004
#&gt; GSM559403     1  0.3373    0.66112 0.744 0.000 0.00 0.008 0.0 0.248
#&gt; GSM559404     1  0.5208    0.39745 0.604 0.000 0.00 0.248 0.0 0.148
#&gt; GSM559405     1  0.2968    0.68499 0.816 0.000 0.00 0.016 0.0 0.168
#&gt; GSM559406     1  0.3431    0.60388 0.756 0.000 0.00 0.228 0.0 0.016
#&gt; GSM559407     1  0.0508    0.71641 0.984 0.000 0.00 0.012 0.0 0.004
#&gt; GSM559408     1  0.0935    0.71279 0.964 0.000 0.00 0.032 0.0 0.004
#&gt; GSM559409     1  0.0935    0.71279 0.964 0.000 0.00 0.032 0.0 0.004
#&gt; GSM559410     1  0.3445    0.66410 0.744 0.000 0.00 0.012 0.0 0.244
#&gt; GSM559411     1  0.1713    0.70035 0.928 0.000 0.00 0.028 0.0 0.044
#&gt; GSM559412     1  0.4025    0.52077 0.720 0.000 0.00 0.232 0.0 0.048
#&gt; GSM559413     1  0.4025    0.52077 0.720 0.000 0.00 0.232 0.0 0.048
#&gt; GSM559415     1  0.6051   -0.27548 0.396 0.000 0.00 0.344 0.0 0.260
#&gt; GSM559416     4  0.3578    0.51342 0.340 0.000 0.00 0.660 0.0 0.000
#&gt; GSM559417     4  0.3482    0.53826 0.316 0.000 0.00 0.684 0.0 0.000
#&gt; GSM559418     1  0.6051   -0.27548 0.396 0.000 0.00 0.344 0.0 0.260
#&gt; GSM559419     1  0.0260    0.71888 0.992 0.000 0.00 0.000 0.0 0.008
#&gt; GSM559420     1  0.0260    0.71888 0.992 0.000 0.00 0.000 0.0 0.008
#&gt; GSM559421     2  0.3221    0.74239 0.000 0.736 0.00 0.264 0.0 0.000
#&gt; GSM559423     2  0.3360    0.74015 0.000 0.732 0.00 0.264 0.0 0.004
#&gt; GSM559425     2  0.0790    0.49872 0.000 0.968 0.00 0.000 0.0 0.032
#&gt; GSM559426     6  0.4238    0.90251 0.000 0.444 0.00 0.016 0.0 0.540
#&gt; GSM559427     2  0.0790    0.49872 0.000 0.968 0.00 0.000 0.0 0.032
#&gt; GSM559428     6  0.4228    0.94043 0.000 0.392 0.00 0.020 0.0 0.588
#&gt; GSM559429     6  0.4168    0.94678 0.000 0.400 0.00 0.016 0.0 0.584
#&gt; GSM559430     2  0.3151    0.73925 0.000 0.748 0.00 0.252 0.0 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:hclust 47         1.81e-01 2
#> SD:hclust 41         2.26e-08 3
#> SD:hclust 46         6.53e-09 4
#> SD:hclust 43         2.96e-08 5
#> SD:hclust 44         2.32e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.482           0.606       0.774         0.3753 0.502   0.502
#> 3 3 1.000           0.981       0.986         0.6200 0.849   0.710
#> 4 4 0.704           0.629       0.814         0.1622 0.931   0.824
#> 5 5 0.657           0.589       0.722         0.0834 0.923   0.776
#> 6 6 0.669           0.577       0.705         0.0493 0.848   0.513
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.9983      0.242 0.476 0.524
#&gt; GSM559387     2  0.9983      0.242 0.476 0.524
#&gt; GSM559391     2  0.9983      0.242 0.476 0.524
#&gt; GSM559395     2  0.9983      0.242 0.476 0.524
#&gt; GSM559397     2  0.9983      0.242 0.476 0.524
#&gt; GSM559401     2  0.9983      0.242 0.476 0.524
#&gt; GSM559414     2  0.9983      0.242 0.476 0.524
#&gt; GSM559422     2  0.9983      0.242 0.476 0.524
#&gt; GSM559424     2  0.9983      0.242 0.476 0.524
#&gt; GSM559431     2  0.9996      0.347 0.488 0.512
#&gt; GSM559432     2  0.0000      0.331 0.000 1.000
#&gt; GSM559381     1  0.0000      0.923 1.000 0.000
#&gt; GSM559382     2  0.9996      0.347 0.488 0.512
#&gt; GSM559384     1  0.0000      0.923 1.000 0.000
#&gt; GSM559385     1  0.0000      0.923 1.000 0.000
#&gt; GSM559386     1  0.4939      0.731 0.892 0.108
#&gt; GSM559388     1  0.9996     -0.349 0.512 0.488
#&gt; GSM559389     1  0.0000      0.923 1.000 0.000
#&gt; GSM559390     1  0.0000      0.923 1.000 0.000
#&gt; GSM559392     2  0.9998      0.339 0.492 0.508
#&gt; GSM559393     1  0.1633      0.889 0.976 0.024
#&gt; GSM559394     1  0.0000      0.923 1.000 0.000
#&gt; GSM559396     1  0.0000      0.923 1.000 0.000
#&gt; GSM559398     2  0.9996      0.347 0.488 0.512
#&gt; GSM559399     1  0.0000      0.923 1.000 0.000
#&gt; GSM559400     1  0.9970     -0.312 0.532 0.468
#&gt; GSM559402     1  0.0000      0.923 1.000 0.000
#&gt; GSM559403     1  0.0000      0.923 1.000 0.000
#&gt; GSM559404     1  0.0672      0.914 0.992 0.008
#&gt; GSM559405     1  0.0000      0.923 1.000 0.000
#&gt; GSM559406     1  0.0672      0.914 0.992 0.008
#&gt; GSM559407     1  0.0000      0.923 1.000 0.000
#&gt; GSM559408     1  0.0000      0.923 1.000 0.000
#&gt; GSM559409     1  0.0000      0.923 1.000 0.000
#&gt; GSM559410     1  0.0000      0.923 1.000 0.000
#&gt; GSM559411     1  0.0672      0.914 0.992 0.008
#&gt; GSM559412     1  0.0672      0.914 0.992 0.008
#&gt; GSM559413     1  0.0672      0.914 0.992 0.008
#&gt; GSM559415     1  0.0000      0.923 1.000 0.000
#&gt; GSM559416     1  0.0000      0.923 1.000 0.000
#&gt; GSM559417     1  0.0000      0.923 1.000 0.000
#&gt; GSM559418     1  0.2603      0.856 0.956 0.044
#&gt; GSM559419     1  0.0000      0.923 1.000 0.000
#&gt; GSM559420     1  0.0000      0.923 1.000 0.000
#&gt; GSM559421     2  0.9996      0.347 0.488 0.512
#&gt; GSM559423     2  0.9996      0.347 0.488 0.512
#&gt; GSM559425     2  0.9996      0.347 0.488 0.512
#&gt; GSM559426     2  0.9996      0.347 0.488 0.512
#&gt; GSM559427     2  0.9996      0.347 0.488 0.512
#&gt; GSM559428     2  0.9996      0.347 0.488 0.512
#&gt; GSM559429     2  0.9996      0.347 0.488 0.512
#&gt; GSM559430     2  0.9996      0.347 0.488 0.512
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559387     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559391     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559395     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559397     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559401     3  0.1399      0.991 0.028 0.004 0.968
#&gt; GSM559414     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559422     3  0.0661      0.974 0.008 0.004 0.988
#&gt; GSM559424     3  0.1163      0.992 0.028 0.000 0.972
#&gt; GSM559431     2  0.0661      0.971 0.004 0.988 0.008
#&gt; GSM559432     3  0.0237      0.966 0.000 0.004 0.996
#&gt; GSM559381     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559382     2  0.1129      0.968 0.004 0.976 0.020
#&gt; GSM559384     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559386     1  0.1315      0.973 0.972 0.008 0.020
#&gt; GSM559388     2  0.1129      0.968 0.004 0.976 0.020
#&gt; GSM559389     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559392     2  0.0237      0.972 0.004 0.996 0.000
#&gt; GSM559393     1  0.1315      0.973 0.972 0.008 0.020
#&gt; GSM559394     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559396     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559398     2  0.0661      0.971 0.004 0.988 0.008
#&gt; GSM559399     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559400     2  0.5200      0.734 0.184 0.796 0.020
#&gt; GSM559402     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559404     1  0.0237      0.992 0.996 0.000 0.004
#&gt; GSM559405     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559417     1  0.0983      0.980 0.980 0.004 0.016
#&gt; GSM559418     1  0.1170      0.976 0.976 0.008 0.016
#&gt; GSM559419     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.996 1.000 0.000 0.000
#&gt; GSM559421     2  0.0237      0.972 0.004 0.996 0.000
#&gt; GSM559423     2  0.0983      0.969 0.004 0.980 0.016
#&gt; GSM559425     2  0.0661      0.971 0.004 0.988 0.008
#&gt; GSM559426     2  0.0237      0.972 0.004 0.996 0.000
#&gt; GSM559427     2  0.0661      0.971 0.004 0.988 0.008
#&gt; GSM559428     2  0.1129      0.968 0.004 0.976 0.020
#&gt; GSM559429     2  0.0983      0.969 0.004 0.980 0.016
#&gt; GSM559430     2  0.0661      0.971 0.004 0.988 0.008
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0188     0.9647 0.000 0.000 0.996 0.004
#&gt; GSM559387     3  0.0000     0.9654 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0188     0.9647 0.000 0.000 0.996 0.004
#&gt; GSM559395     3  0.0000     0.9654 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000     0.9654 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0000     0.9654 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000     0.9654 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.3649     0.8581 0.000 0.000 0.796 0.204
#&gt; GSM559424     3  0.0188     0.9647 0.000 0.000 0.996 0.004
#&gt; GSM559431     2  0.0336     0.8567 0.000 0.992 0.000 0.008
#&gt; GSM559432     3  0.3726     0.8528 0.000 0.000 0.788 0.212
#&gt; GSM559381     1  0.1792     0.6394 0.932 0.000 0.000 0.068
#&gt; GSM559382     2  0.5771     0.4645 0.028 0.512 0.000 0.460
#&gt; GSM559384     1  0.2216     0.6468 0.908 0.000 0.000 0.092
#&gt; GSM559385     1  0.1792     0.6321 0.932 0.000 0.000 0.068
#&gt; GSM559386     1  0.4907    -0.0738 0.580 0.000 0.000 0.420
#&gt; GSM559388     2  0.4907     0.5400 0.000 0.580 0.000 0.420
#&gt; GSM559389     1  0.2081     0.6182 0.916 0.000 0.000 0.084
#&gt; GSM559390     4  0.4843     0.4317 0.396 0.000 0.000 0.604
#&gt; GSM559392     2  0.1867     0.8612 0.000 0.928 0.000 0.072
#&gt; GSM559393     1  0.4948    -0.0473 0.560 0.000 0.000 0.440
#&gt; GSM559394     1  0.1792     0.6321 0.932 0.000 0.000 0.068
#&gt; GSM559396     1  0.4222     0.5020 0.728 0.000 0.000 0.272
#&gt; GSM559398     2  0.0336     0.8589 0.000 0.992 0.000 0.008
#&gt; GSM559399     1  0.2408     0.5873 0.896 0.000 0.000 0.104
#&gt; GSM559400     4  0.5947    -0.3058 0.044 0.384 0.000 0.572
#&gt; GSM559402     1  0.2868     0.6303 0.864 0.000 0.000 0.136
#&gt; GSM559403     1  0.1792     0.6321 0.932 0.000 0.000 0.068
#&gt; GSM559404     1  0.2081     0.6371 0.916 0.000 0.000 0.084
#&gt; GSM559405     1  0.0000     0.6533 1.000 0.000 0.000 0.000
#&gt; GSM559406     1  0.4356     0.4772 0.708 0.000 0.000 0.292
#&gt; GSM559407     1  0.2921     0.6288 0.860 0.000 0.000 0.140
#&gt; GSM559408     1  0.4277     0.4927 0.720 0.000 0.000 0.280
#&gt; GSM559409     1  0.4277     0.4927 0.720 0.000 0.000 0.280
#&gt; GSM559410     1  0.0336     0.6537 0.992 0.000 0.000 0.008
#&gt; GSM559411     1  0.4356     0.4871 0.708 0.000 0.000 0.292
#&gt; GSM559412     1  0.4356     0.4871 0.708 0.000 0.000 0.292
#&gt; GSM559413     1  0.4356     0.4871 0.708 0.000 0.000 0.292
#&gt; GSM559415     1  0.2345     0.5923 0.900 0.000 0.000 0.100
#&gt; GSM559416     4  0.4961     0.4070 0.448 0.000 0.000 0.552
#&gt; GSM559417     4  0.4961     0.4070 0.448 0.000 0.000 0.552
#&gt; GSM559418     1  0.4477     0.1311 0.688 0.000 0.000 0.312
#&gt; GSM559419     1  0.4888     0.0463 0.588 0.000 0.000 0.412
#&gt; GSM559420     1  0.2868     0.6303 0.864 0.000 0.000 0.136
#&gt; GSM559421     2  0.1792     0.8616 0.000 0.932 0.000 0.068
#&gt; GSM559423     2  0.2011     0.8592 0.000 0.920 0.000 0.080
#&gt; GSM559425     2  0.0000     0.8582 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.2011     0.8598 0.000 0.920 0.000 0.080
#&gt; GSM559427     2  0.0000     0.8582 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.5774     0.4681 0.028 0.508 0.000 0.464
#&gt; GSM559429     2  0.2345     0.8551 0.000 0.900 0.000 0.100
#&gt; GSM559430     2  0.0000     0.8582 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM559383     3  0.0898     0.9102 0.000 0.000 0.972 0.020 NA
#&gt; GSM559387     3  0.0000     0.9145 0.000 0.000 1.000 0.000 NA
#&gt; GSM559391     3  0.0898     0.9102 0.000 0.000 0.972 0.020 NA
#&gt; GSM559395     3  0.0000     0.9145 0.000 0.000 1.000 0.000 NA
#&gt; GSM559397     3  0.0000     0.9145 0.000 0.000 1.000 0.000 NA
#&gt; GSM559401     3  0.0000     0.9145 0.000 0.000 1.000 0.000 NA
#&gt; GSM559414     3  0.0000     0.9145 0.000 0.000 1.000 0.000 NA
#&gt; GSM559422     3  0.4256     0.6363 0.000 0.000 0.564 0.000 NA
#&gt; GSM559424     3  0.0898     0.9102 0.000 0.000 0.972 0.020 NA
#&gt; GSM559431     2  0.0324     0.9059 0.000 0.992 0.000 0.004 NA
#&gt; GSM559432     3  0.4268     0.6293 0.000 0.000 0.556 0.000 NA
#&gt; GSM559381     1  0.3532     0.5857 0.832 0.000 0.000 0.092 NA
#&gt; GSM559382     4  0.7126     0.2497 0.032 0.304 0.000 0.468 NA
#&gt; GSM559384     1  0.3543     0.6114 0.828 0.000 0.000 0.112 NA
#&gt; GSM559385     1  0.3565     0.5580 0.816 0.000 0.000 0.040 NA
#&gt; GSM559386     4  0.6372     0.2367 0.376 0.000 0.000 0.456 NA
#&gt; GSM559388     4  0.6553     0.1365 0.004 0.368 0.000 0.452 NA
#&gt; GSM559389     1  0.3242     0.5672 0.852 0.000 0.000 0.076 NA
#&gt; GSM559390     4  0.3532     0.3905 0.128 0.000 0.000 0.824 NA
#&gt; GSM559392     2  0.3184     0.8779 0.000 0.852 0.000 0.048 NA
#&gt; GSM559393     1  0.6361     0.0439 0.484 0.000 0.000 0.340 NA
#&gt; GSM559394     1  0.3803     0.5557 0.804 0.000 0.000 0.056 NA
#&gt; GSM559396     1  0.5727     0.4091 0.560 0.000 0.000 0.340 NA
#&gt; GSM559398     2  0.1549     0.9009 0.000 0.944 0.000 0.016 NA
#&gt; GSM559399     1  0.3639     0.5319 0.812 0.000 0.000 0.144 NA
#&gt; GSM559400     4  0.5605     0.4411 0.012 0.172 0.000 0.672 NA
#&gt; GSM559402     1  0.3930     0.5978 0.792 0.000 0.000 0.152 NA
#&gt; GSM559403     1  0.3262     0.5685 0.840 0.000 0.000 0.036 NA
#&gt; GSM559404     1  0.4303     0.5598 0.752 0.000 0.000 0.056 NA
#&gt; GSM559405     1  0.0451     0.6184 0.988 0.000 0.000 0.004 NA
#&gt; GSM559406     1  0.5447     0.3723 0.500 0.000 0.000 0.440 NA
#&gt; GSM559407     1  0.4049     0.5947 0.780 0.000 0.000 0.164 NA
#&gt; GSM559408     1  0.5622     0.3926 0.508 0.000 0.000 0.416 NA
#&gt; GSM559409     1  0.5703     0.3967 0.508 0.000 0.000 0.408 NA
#&gt; GSM559410     1  0.1403     0.6188 0.952 0.000 0.000 0.024 NA
#&gt; GSM559411     1  0.5928     0.4046 0.500 0.000 0.000 0.392 NA
#&gt; GSM559412     1  0.5826     0.3915 0.500 0.000 0.000 0.404 NA
#&gt; GSM559413     1  0.6019     0.4019 0.500 0.000 0.000 0.380 NA
#&gt; GSM559415     1  0.3595     0.5388 0.816 0.000 0.000 0.140 NA
#&gt; GSM559416     4  0.3231     0.3031 0.196 0.000 0.000 0.800 NA
#&gt; GSM559417     4  0.3003     0.3194 0.188 0.000 0.000 0.812 NA
#&gt; GSM559418     1  0.5145     0.1985 0.612 0.000 0.000 0.332 NA
#&gt; GSM559419     4  0.4686    -0.1650 0.384 0.000 0.000 0.596 NA
#&gt; GSM559420     1  0.4190     0.5938 0.768 0.000 0.000 0.172 NA
#&gt; GSM559421     2  0.2735     0.8927 0.000 0.880 0.000 0.036 NA
#&gt; GSM559423     2  0.3390     0.8749 0.000 0.840 0.000 0.060 NA
#&gt; GSM559425     2  0.0000     0.9077 0.000 1.000 0.000 0.000 NA
#&gt; GSM559426     2  0.3276     0.8659 0.000 0.836 0.000 0.032 NA
#&gt; GSM559427     2  0.0000     0.9077 0.000 1.000 0.000 0.000 NA
#&gt; GSM559428     4  0.7328     0.1986 0.028 0.292 0.000 0.408 NA
#&gt; GSM559429     2  0.4252     0.8074 0.000 0.764 0.000 0.064 NA
#&gt; GSM559430     2  0.0000     0.9077 0.000 1.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.1151     0.9487 0.012 0.000 0.956 0.000 0.000 0.032
#&gt; GSM559387     3  0.0000     0.9679 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.1151     0.9487 0.012 0.000 0.956 0.000 0.000 0.032
#&gt; GSM559395     3  0.0000     0.9679 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000     0.9679 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0291     0.9614 0.004 0.000 0.992 0.000 0.004 0.000
#&gt; GSM559414     3  0.0000     0.9679 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.3817     0.9749 0.000 0.000 0.432 0.000 0.568 0.000
#&gt; GSM559424     3  0.1151     0.9487 0.012 0.000 0.956 0.000 0.000 0.032
#&gt; GSM559431     2  0.1232     0.7825 0.024 0.956 0.000 0.000 0.016 0.004
#&gt; GSM559432     5  0.4032     0.9753 0.000 0.000 0.420 0.000 0.572 0.008
#&gt; GSM559381     1  0.6238     0.1789 0.452 0.000 0.000 0.392 0.056 0.100
#&gt; GSM559382     6  0.3837     0.5939 0.068 0.140 0.000 0.000 0.008 0.784
#&gt; GSM559384     4  0.5351     0.0571 0.388 0.000 0.000 0.528 0.064 0.020
#&gt; GSM559385     1  0.5380     0.5903 0.660 0.000 0.000 0.192 0.104 0.044
#&gt; GSM559386     6  0.3826     0.5436 0.236 0.000 0.000 0.012 0.016 0.736
#&gt; GSM559388     6  0.3727     0.5380 0.040 0.188 0.000 0.000 0.004 0.768
#&gt; GSM559389     1  0.4378     0.6229 0.704 0.000 0.000 0.240 0.016 0.040
#&gt; GSM559390     6  0.6037     0.0252 0.068 0.000 0.000 0.408 0.064 0.460
#&gt; GSM559392     2  0.3314     0.7102 0.000 0.740 0.000 0.000 0.004 0.256
#&gt; GSM559393     1  0.4754     0.4537 0.692 0.000 0.000 0.016 0.080 0.212
#&gt; GSM559394     1  0.4702     0.6190 0.724 0.000 0.000 0.164 0.080 0.032
#&gt; GSM559396     4  0.6695     0.2695 0.256 0.000 0.000 0.504 0.100 0.140
#&gt; GSM559398     2  0.1908     0.7695 0.000 0.900 0.000 0.000 0.004 0.096
#&gt; GSM559399     1  0.4740     0.6034 0.696 0.000 0.000 0.220 0.032 0.052
#&gt; GSM559400     6  0.5131     0.5924 0.052 0.060 0.000 0.084 0.060 0.744
#&gt; GSM559402     4  0.5017     0.2282 0.312 0.000 0.000 0.612 0.060 0.016
#&gt; GSM559403     1  0.4837     0.6116 0.692 0.000 0.000 0.208 0.076 0.024
#&gt; GSM559404     4  0.5828    -0.1455 0.356 0.000 0.000 0.516 0.096 0.032
#&gt; GSM559405     1  0.4010     0.4571 0.584 0.000 0.000 0.408 0.008 0.000
#&gt; GSM559406     4  0.2924     0.5319 0.024 0.000 0.000 0.868 0.040 0.068
#&gt; GSM559407     4  0.4986     0.2411 0.304 0.000 0.000 0.620 0.060 0.016
#&gt; GSM559408     4  0.1464     0.5577 0.016 0.000 0.000 0.944 0.004 0.036
#&gt; GSM559409     4  0.1148     0.5601 0.016 0.000 0.000 0.960 0.004 0.020
#&gt; GSM559410     1  0.4200     0.4994 0.592 0.000 0.000 0.392 0.012 0.004
#&gt; GSM559411     4  0.1307     0.5543 0.008 0.000 0.000 0.952 0.032 0.008
#&gt; GSM559412     4  0.0914     0.5591 0.000 0.000 0.000 0.968 0.016 0.016
#&gt; GSM559413     4  0.0777     0.5579 0.000 0.000 0.000 0.972 0.024 0.004
#&gt; GSM559415     1  0.4700     0.6053 0.704 0.000 0.000 0.212 0.040 0.044
#&gt; GSM559416     4  0.6855     0.1138 0.152 0.000 0.000 0.448 0.092 0.308
#&gt; GSM559417     4  0.6830     0.0855 0.144 0.000 0.000 0.444 0.092 0.320
#&gt; GSM559418     1  0.4922     0.5507 0.712 0.000 0.000 0.096 0.040 0.152
#&gt; GSM559419     4  0.6340     0.3407 0.236 0.000 0.000 0.548 0.068 0.148
#&gt; GSM559420     4  0.5407     0.2150 0.340 0.000 0.000 0.564 0.072 0.024
#&gt; GSM559421     2  0.3189     0.7244 0.000 0.760 0.000 0.000 0.004 0.236
#&gt; GSM559423     2  0.4525     0.6596 0.020 0.672 0.000 0.000 0.032 0.276
#&gt; GSM559425     2  0.0820     0.7859 0.016 0.972 0.000 0.000 0.012 0.000
#&gt; GSM559426     2  0.5434     0.6489 0.052 0.664 0.000 0.000 0.112 0.172
#&gt; GSM559427     2  0.0820     0.7859 0.016 0.972 0.000 0.000 0.012 0.000
#&gt; GSM559428     6  0.6158     0.4450 0.112 0.132 0.000 0.000 0.152 0.604
#&gt; GSM559429     2  0.6369     0.4752 0.068 0.536 0.000 0.000 0.140 0.256
#&gt; GSM559430     2  0.0000     0.7875 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:kmeans 28               NA 2
#> SD:kmeans 52         8.27e-11 3
#> SD:kmeans 36         1.30e-07 4
#> SD:kmeans 34         3.24e-07 5
#> SD:kmeans 36         6.48e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.571           0.918       0.933         0.4814 0.517   0.517
#> 3 3 1.000           0.982       0.993         0.3439 0.773   0.586
#> 4 4 0.866           0.932       0.954         0.1661 0.851   0.600
#> 5 5 0.804           0.698       0.839         0.0574 0.907   0.658
#> 6 6 0.781           0.547       0.789         0.0331 0.965   0.844
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.184      0.923 0.972 0.028
#&gt; GSM559387     1   0.184      0.923 0.972 0.028
#&gt; GSM559391     1   0.184      0.923 0.972 0.028
#&gt; GSM559395     1   0.184      0.923 0.972 0.028
#&gt; GSM559397     1   0.184      0.923 0.972 0.028
#&gt; GSM559401     1   0.184      0.923 0.972 0.028
#&gt; GSM559414     1   0.184      0.923 0.972 0.028
#&gt; GSM559422     2   0.871      0.656 0.292 0.708
#&gt; GSM559424     1   0.184      0.923 0.972 0.028
#&gt; GSM559431     2   0.000      0.946 0.000 1.000
#&gt; GSM559432     2   0.518      0.857 0.116 0.884
#&gt; GSM559381     1   0.662      0.863 0.828 0.172
#&gt; GSM559382     2   0.118      0.954 0.016 0.984
#&gt; GSM559384     1   0.518      0.914 0.884 0.116
#&gt; GSM559385     1   0.518      0.914 0.884 0.116
#&gt; GSM559386     2   0.184      0.947 0.028 0.972
#&gt; GSM559388     2   0.118      0.954 0.016 0.984
#&gt; GSM559389     1   0.662      0.863 0.828 0.172
#&gt; GSM559390     1   0.000      0.930 1.000 0.000
#&gt; GSM559392     2   0.118      0.954 0.016 0.984
#&gt; GSM559393     2   0.184      0.947 0.028 0.972
#&gt; GSM559394     1   0.518      0.914 0.884 0.116
#&gt; GSM559396     1   0.118      0.924 0.984 0.016
#&gt; GSM559398     2   0.118      0.954 0.016 0.984
#&gt; GSM559399     1   0.518      0.914 0.884 0.116
#&gt; GSM559400     2   0.518      0.857 0.116 0.884
#&gt; GSM559402     1   0.518      0.914 0.884 0.116
#&gt; GSM559403     1   0.518      0.914 0.884 0.116
#&gt; GSM559404     1   0.000      0.930 1.000 0.000
#&gt; GSM559405     1   0.518      0.914 0.884 0.116
#&gt; GSM559406     1   0.000      0.930 1.000 0.000
#&gt; GSM559407     1   0.518      0.914 0.884 0.116
#&gt; GSM559408     1   0.163      0.931 0.976 0.024
#&gt; GSM559409     1   0.118      0.931 0.984 0.016
#&gt; GSM559410     1   0.518      0.914 0.884 0.116
#&gt; GSM559411     1   0.000      0.930 1.000 0.000
#&gt; GSM559412     1   0.000      0.930 1.000 0.000
#&gt; GSM559413     1   0.000      0.930 1.000 0.000
#&gt; GSM559415     1   0.518      0.914 0.884 0.116
#&gt; GSM559416     1   0.260      0.927 0.956 0.044
#&gt; GSM559417     2   0.795      0.746 0.240 0.760
#&gt; GSM559418     2   0.184      0.947 0.028 0.972
#&gt; GSM559419     1   0.469      0.919 0.900 0.100
#&gt; GSM559420     1   0.469      0.919 0.900 0.100
#&gt; GSM559421     2   0.118      0.954 0.016 0.984
#&gt; GSM559423     2   0.118      0.954 0.016 0.984
#&gt; GSM559425     2   0.118      0.954 0.016 0.984
#&gt; GSM559426     2   0.118      0.954 0.016 0.984
#&gt; GSM559427     2   0.118      0.954 0.016 0.984
#&gt; GSM559428     2   0.000      0.946 0.000 1.000
#&gt; GSM559429     2   0.000      0.946 0.000 1.000
#&gt; GSM559430     2   0.118      0.954 0.016 0.984
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559432     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559381     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559382     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559384     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559386     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559388     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559392     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559393     2  0.0592      0.967 0.012 0.988 0.000
#&gt; GSM559394     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559396     3  0.0237      0.995 0.004 0.000 0.996
#&gt; GSM559398     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559400     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559402     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559417     1  0.3038      0.880 0.896 0.104 0.000
#&gt; GSM559418     2  0.5178      0.654 0.256 0.744 0.000
#&gt; GSM559419     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559428     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559429     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      0.980 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559424     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; GSM559431     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559432     3  0.0469      0.985 0.000 0.012 0.988 0.000
#&gt; GSM559381     1  0.1389      0.898 0.952 0.000 0.000 0.048
#&gt; GSM559382     2  0.0188      0.979 0.000 0.996 0.000 0.004
#&gt; GSM559384     1  0.2149      0.878 0.912 0.000 0.000 0.088
#&gt; GSM559385     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; GSM559386     2  0.3164      0.884 0.064 0.884 0.000 0.052
#&gt; GSM559388     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559389     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0921      0.923 0.028 0.000 0.000 0.972
#&gt; GSM559392     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.2053      0.856 0.924 0.072 0.000 0.004
#&gt; GSM559394     1  0.0469      0.898 0.988 0.000 0.000 0.012
#&gt; GSM559396     3  0.0817      0.974 0.000 0.000 0.976 0.024
#&gt; GSM559398     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.2589      0.859 0.884 0.000 0.000 0.116
#&gt; GSM559400     2  0.2921      0.839 0.000 0.860 0.000 0.140
#&gt; GSM559402     1  0.3649      0.772 0.796 0.000 0.000 0.204
#&gt; GSM559403     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.1022      0.901 0.968 0.000 0.000 0.032
#&gt; GSM559405     1  0.1022      0.901 0.968 0.000 0.000 0.032
#&gt; GSM559406     4  0.2345      0.943 0.100 0.000 0.000 0.900
#&gt; GSM559407     1  0.3837      0.747 0.776 0.000 0.000 0.224
#&gt; GSM559408     4  0.2469      0.943 0.108 0.000 0.000 0.892
#&gt; GSM559409     4  0.2530      0.943 0.112 0.000 0.000 0.888
#&gt; GSM559410     1  0.1557      0.897 0.944 0.000 0.000 0.056
#&gt; GSM559411     4  0.2530      0.943 0.112 0.000 0.000 0.888
#&gt; GSM559412     4  0.2530      0.943 0.112 0.000 0.000 0.888
#&gt; GSM559413     4  0.2530      0.943 0.112 0.000 0.000 0.888
#&gt; GSM559415     1  0.2704      0.858 0.876 0.000 0.000 0.124
#&gt; GSM559416     4  0.0000      0.912 0.000 0.000 0.000 1.000
#&gt; GSM559417     4  0.0000      0.912 0.000 0.000 0.000 1.000
#&gt; GSM559418     1  0.3497      0.839 0.860 0.036 0.000 0.104
#&gt; GSM559419     4  0.0336      0.916 0.008 0.000 0.000 0.992
#&gt; GSM559420     1  0.4164      0.709 0.736 0.000 0.000 0.264
#&gt; GSM559421     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559429     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559430     2  0.0000      0.982 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.0579     0.9440 0.008 0.000 0.984 0.008 0.000
#&gt; GSM559424     3  0.0000     0.9529 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     3  0.1770     0.8974 0.008 0.048 0.936 0.008 0.000
#&gt; GSM559381     1  0.4306     0.3818 0.660 0.000 0.000 0.012 0.328
#&gt; GSM559382     2  0.1828     0.9066 0.028 0.936 0.000 0.004 0.032
#&gt; GSM559384     1  0.2930     0.5626 0.832 0.000 0.000 0.004 0.164
#&gt; GSM559385     5  0.1341     0.7783 0.056 0.000 0.000 0.000 0.944
#&gt; GSM559386     2  0.6358     0.5633 0.064 0.632 0.000 0.104 0.200
#&gt; GSM559388     2  0.1372     0.9188 0.024 0.956 0.000 0.004 0.016
#&gt; GSM559389     5  0.2719     0.7417 0.144 0.000 0.000 0.004 0.852
#&gt; GSM559390     4  0.3495     0.6639 0.152 0.000 0.000 0.816 0.032
#&gt; GSM559392     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559393     5  0.1082     0.7513 0.028 0.008 0.000 0.000 0.964
#&gt; GSM559394     5  0.1197     0.7786 0.048 0.000 0.000 0.000 0.952
#&gt; GSM559396     3  0.4478     0.4816 0.360 0.000 0.628 0.004 0.008
#&gt; GSM559398     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     5  0.5405     0.7064 0.136 0.000 0.000 0.204 0.660
#&gt; GSM559400     2  0.4387     0.4867 0.012 0.640 0.000 0.348 0.000
#&gt; GSM559402     1  0.2020     0.5849 0.900 0.000 0.000 0.000 0.100
#&gt; GSM559403     5  0.2020     0.7713 0.100 0.000 0.000 0.000 0.900
#&gt; GSM559404     1  0.4470     0.3447 0.616 0.000 0.000 0.012 0.372
#&gt; GSM559405     1  0.4287    -0.0388 0.540 0.000 0.000 0.000 0.460
#&gt; GSM559406     4  0.4225     0.2894 0.364 0.000 0.000 0.632 0.004
#&gt; GSM559407     1  0.2193     0.5848 0.900 0.000 0.000 0.008 0.092
#&gt; GSM559408     1  0.4450    -0.0351 0.508 0.000 0.000 0.488 0.004
#&gt; GSM559409     1  0.4464     0.1714 0.584 0.000 0.000 0.408 0.008
#&gt; GSM559410     5  0.4872     0.2309 0.436 0.000 0.000 0.024 0.540
#&gt; GSM559411     1  0.3534     0.4101 0.744 0.000 0.000 0.256 0.000
#&gt; GSM559412     1  0.4350     0.2057 0.588 0.000 0.000 0.408 0.004
#&gt; GSM559413     1  0.4084     0.3389 0.668 0.000 0.000 0.328 0.004
#&gt; GSM559415     5  0.5164     0.7098 0.096 0.000 0.000 0.232 0.672
#&gt; GSM559416     4  0.0794     0.7313 0.028 0.000 0.000 0.972 0.000
#&gt; GSM559417     4  0.0865     0.7307 0.024 0.004 0.000 0.972 0.000
#&gt; GSM559418     5  0.5324     0.6988 0.072 0.016 0.000 0.232 0.680
#&gt; GSM559419     4  0.3882     0.5705 0.224 0.000 0.000 0.756 0.020
#&gt; GSM559420     1  0.3906     0.5014 0.800 0.000 0.000 0.132 0.068
#&gt; GSM559421     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0162     0.9370 0.004 0.996 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.1483     0.9192 0.028 0.952 0.000 0.012 0.008
#&gt; GSM559429     2  0.0833     0.9288 0.016 0.976 0.000 0.004 0.004
#&gt; GSM559430     2  0.0000     0.9382 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0146   0.914801 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559387     3  0.0000   0.915374 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0146   0.914801 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559395     3  0.0000   0.915374 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000   0.915374 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0146   0.914136 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559414     3  0.0000   0.915374 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     3  0.2145   0.859434 0.012 0.000 0.904 0.004 0.004 0.076
#&gt; GSM559424     3  0.0146   0.914801 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559431     2  0.0000   0.875174 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     3  0.3331   0.803246 0.008 0.060 0.844 0.004 0.004 0.080
#&gt; GSM559381     6  0.6042   0.000000 0.212 0.004 0.000 0.360 0.000 0.424
#&gt; GSM559382     2  0.3264   0.788116 0.012 0.796 0.000 0.000 0.008 0.184
#&gt; GSM559384     4  0.5269  -0.247684 0.088 0.000 0.000 0.604 0.016 0.292
#&gt; GSM559385     1  0.1168   0.628381 0.956 0.000 0.000 0.016 0.000 0.028
#&gt; GSM559386     2  0.6950   0.195084 0.136 0.412 0.000 0.008 0.080 0.364
#&gt; GSM559388     2  0.2333   0.828547 0.004 0.872 0.000 0.000 0.004 0.120
#&gt; GSM559389     1  0.4300   0.384322 0.712 0.000 0.000 0.080 0.000 0.208
#&gt; GSM559390     5  0.4730   0.532472 0.008 0.000 0.000 0.184 0.696 0.112
#&gt; GSM559392     2  0.0790   0.871559 0.000 0.968 0.000 0.000 0.000 0.032
#&gt; GSM559393     1  0.1765   0.582994 0.904 0.000 0.000 0.000 0.000 0.096
#&gt; GSM559394     1  0.0748   0.640108 0.976 0.000 0.000 0.016 0.004 0.004
#&gt; GSM559396     3  0.6332  -0.076423 0.000 0.000 0.408 0.264 0.012 0.316
#&gt; GSM559398     2  0.0632   0.873444 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; GSM559399     1  0.6681   0.461879 0.508 0.000 0.000 0.080 0.204 0.208
#&gt; GSM559400     2  0.5653   0.216136 0.000 0.496 0.000 0.004 0.360 0.140
#&gt; GSM559402     4  0.3351   0.076968 0.040 0.000 0.000 0.800 0.000 0.160
#&gt; GSM559403     1  0.1950   0.613654 0.912 0.000 0.000 0.064 0.000 0.024
#&gt; GSM559404     4  0.5027  -0.049062 0.324 0.000 0.000 0.600 0.012 0.064
#&gt; GSM559405     4  0.5681  -0.337238 0.416 0.000 0.000 0.428 0.000 0.156
#&gt; GSM559406     4  0.4472  -0.000703 0.000 0.000 0.000 0.496 0.476 0.028
#&gt; GSM559407     4  0.3457   0.125477 0.052 0.000 0.000 0.808 0.004 0.136
#&gt; GSM559408     4  0.4254   0.311889 0.004 0.000 0.000 0.624 0.352 0.020
#&gt; GSM559409     4  0.4255   0.408131 0.012 0.000 0.000 0.680 0.284 0.024
#&gt; GSM559410     4  0.5816  -0.183573 0.428 0.000 0.000 0.432 0.012 0.128
#&gt; GSM559411     4  0.3381   0.399554 0.000 0.000 0.000 0.800 0.156 0.044
#&gt; GSM559412     4  0.3650   0.424069 0.004 0.000 0.000 0.716 0.272 0.008
#&gt; GSM559413     4  0.2902   0.425151 0.000 0.000 0.000 0.800 0.196 0.004
#&gt; GSM559415     1  0.6152   0.523638 0.548 0.000 0.000 0.044 0.256 0.152
#&gt; GSM559416     5  0.0806   0.766677 0.000 0.000 0.000 0.020 0.972 0.008
#&gt; GSM559417     5  0.0993   0.766864 0.000 0.000 0.000 0.024 0.964 0.012
#&gt; GSM559418     1  0.6701   0.487105 0.496 0.020 0.000 0.040 0.292 0.152
#&gt; GSM559419     5  0.5041   0.551543 0.024 0.000 0.000 0.128 0.688 0.160
#&gt; GSM559420     4  0.5766  -0.211403 0.036 0.000 0.000 0.572 0.104 0.288
#&gt; GSM559421     2  0.0632   0.873954 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; GSM559423     2  0.0260   0.874955 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559425     2  0.0000   0.875174 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0790   0.867898 0.000 0.968 0.000 0.000 0.000 0.032
#&gt; GSM559427     2  0.0000   0.875174 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.3240   0.732849 0.000 0.752 0.000 0.000 0.004 0.244
#&gt; GSM559429     2  0.2092   0.823455 0.000 0.876 0.000 0.000 0.000 0.124
#&gt; GSM559430     2  0.0000   0.875174 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> SD:skmeans 52         6.10e-01 2
#> SD:skmeans 52         1.31e-09 3
#> SD:skmeans 52         6.69e-09 4
#> SD:skmeans 40         3.95e-07 5
#> SD:skmeans 32         3.80e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.966       0.973         0.3268 0.683   0.683
#> 3 3 1.000           0.958       0.985         0.7857 0.729   0.603
#> 4 4 0.855           0.932       0.947         0.2300 0.853   0.652
#> 5 5 0.823           0.902       0.929         0.0234 0.988   0.958
#> 6 6 0.758           0.794       0.871         0.0325 0.981   0.932
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.3114      0.993 0.056 0.944
#&gt; GSM559387     2  0.3114      0.993 0.056 0.944
#&gt; GSM559391     2  0.3114      0.993 0.056 0.944
#&gt; GSM559395     2  0.3114      0.993 0.056 0.944
#&gt; GSM559397     2  0.3114      0.993 0.056 0.944
#&gt; GSM559401     2  0.3114      0.993 0.056 0.944
#&gt; GSM559414     2  0.3114      0.993 0.056 0.944
#&gt; GSM559422     2  0.3114      0.993 0.056 0.944
#&gt; GSM559424     2  0.3114      0.993 0.056 0.944
#&gt; GSM559431     1  0.3114      0.950 0.944 0.056
#&gt; GSM559432     2  0.0000      0.940 0.000 1.000
#&gt; GSM559381     1  0.0000      0.976 1.000 0.000
#&gt; GSM559382     1  0.0000      0.976 1.000 0.000
#&gt; GSM559384     1  0.0000      0.976 1.000 0.000
#&gt; GSM559385     1  0.0000      0.976 1.000 0.000
#&gt; GSM559386     1  0.0000      0.976 1.000 0.000
#&gt; GSM559388     1  0.0376      0.974 0.996 0.004
#&gt; GSM559389     1  0.0000      0.976 1.000 0.000
#&gt; GSM559390     1  0.0000      0.976 1.000 0.000
#&gt; GSM559392     1  0.3114      0.950 0.944 0.056
#&gt; GSM559393     1  0.0000      0.976 1.000 0.000
#&gt; GSM559394     1  0.0000      0.976 1.000 0.000
#&gt; GSM559396     1  0.0000      0.976 1.000 0.000
#&gt; GSM559398     1  0.3114      0.950 0.944 0.056
#&gt; GSM559399     1  0.0000      0.976 1.000 0.000
#&gt; GSM559400     1  0.3114      0.950 0.944 0.056
#&gt; GSM559402     1  0.0000      0.976 1.000 0.000
#&gt; GSM559403     1  0.0000      0.976 1.000 0.000
#&gt; GSM559404     1  0.6801      0.771 0.820 0.180
#&gt; GSM559405     1  0.0000      0.976 1.000 0.000
#&gt; GSM559406     1  0.0672      0.971 0.992 0.008
#&gt; GSM559407     1  0.0000      0.976 1.000 0.000
#&gt; GSM559408     1  0.0000      0.976 1.000 0.000
#&gt; GSM559409     1  0.0000      0.976 1.000 0.000
#&gt; GSM559410     1  0.0000      0.976 1.000 0.000
#&gt; GSM559411     1  0.0000      0.976 1.000 0.000
#&gt; GSM559412     1  0.0000      0.976 1.000 0.000
#&gt; GSM559413     1  0.5178      0.860 0.884 0.116
#&gt; GSM559415     1  0.0000      0.976 1.000 0.000
#&gt; GSM559416     1  0.0000      0.976 1.000 0.000
#&gt; GSM559417     1  0.0000      0.976 1.000 0.000
#&gt; GSM559418     1  0.0000      0.976 1.000 0.000
#&gt; GSM559419     1  0.0000      0.976 1.000 0.000
#&gt; GSM559420     1  0.0000      0.976 1.000 0.000
#&gt; GSM559421     1  0.3114      0.950 0.944 0.056
#&gt; GSM559423     1  0.3114      0.950 0.944 0.056
#&gt; GSM559425     1  0.3114      0.950 0.944 0.056
#&gt; GSM559426     1  0.3114      0.950 0.944 0.056
#&gt; GSM559427     1  0.3114      0.950 0.944 0.056
#&gt; GSM559428     1  0.0000      0.976 1.000 0.000
#&gt; GSM559429     1  0.3114      0.950 0.944 0.056
#&gt; GSM559430     1  0.3114      0.950 0.944 0.056
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3
#&gt; GSM559383     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559387     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559391     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559395     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559397     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559401     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559414     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559422     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559424     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559431     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559432     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559381     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559382     1   0.460      0.726 0.796 0.204  0
#&gt; GSM559384     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559385     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559386     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559388     2   0.619      0.284 0.420 0.580  0
#&gt; GSM559389     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559390     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559392     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559393     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559394     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559396     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559398     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559399     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559400     2   0.341      0.796 0.124 0.876  0
#&gt; GSM559402     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559403     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559404     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559405     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559406     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559407     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559408     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559409     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559410     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559411     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559412     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559413     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559415     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559416     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559417     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559418     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559419     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559420     1   0.000      0.991 1.000 0.000  0
#&gt; GSM559421     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559423     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559425     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559426     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559427     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559428     1   0.116      0.964 0.972 0.028  0
#&gt; GSM559429     2   0.000      0.931 0.000 1.000  0
#&gt; GSM559430     2   0.000      0.931 0.000 1.000  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3    p4
#&gt; GSM559383     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559387     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559391     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559395     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559397     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559401     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559414     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559422     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559424     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559431     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559432     3   0.000      1.000 0.000 0.000  1 0.000
#&gt; GSM559381     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559382     4   0.422      0.859 0.100 0.076  0 0.824
#&gt; GSM559384     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559385     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559386     4   0.241      0.884 0.104 0.000  0 0.896
#&gt; GSM559388     4   0.518      0.817 0.120 0.120  0 0.760
#&gt; GSM559389     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559390     4   0.000      0.887 0.000 0.000  0 1.000
#&gt; GSM559392     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559393     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559394     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559396     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559398     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559399     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559400     4   0.234      0.884 0.100 0.000  0 0.900
#&gt; GSM559402     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559403     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559404     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559405     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559406     4   0.000      0.887 0.000 0.000  0 1.000
#&gt; GSM559407     1   0.121      0.929 0.960 0.000  0 0.040
#&gt; GSM559408     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559409     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559410     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559411     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559412     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559413     1   0.234      0.898 0.900 0.000  0 0.100
#&gt; GSM559415     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559416     4   0.000      0.887 0.000 0.000  0 1.000
#&gt; GSM559417     4   0.000      0.887 0.000 0.000  0 1.000
#&gt; GSM559418     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559419     1   0.496      0.215 0.552 0.000  0 0.448
#&gt; GSM559420     1   0.000      0.946 1.000 0.000  0 0.000
#&gt; GSM559421     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559423     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559425     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559426     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559427     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559428     4   0.433      0.767 0.244 0.008  0 0.748
#&gt; GSM559429     2   0.000      1.000 0.000 1.000  0 0.000
#&gt; GSM559430     2   0.000      1.000 0.000 1.000  0 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     5  0.3452      1.000 0.000 0.000 0.244 0.000 0.756
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.0000      0.942 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.3452      1.000 0.000 0.000 0.244 0.000 0.756
#&gt; GSM559381     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     4  0.5148      0.787 0.100 0.084 0.000 0.752 0.064
#&gt; GSM559384     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559386     4  0.2669      0.826 0.104 0.000 0.000 0.876 0.020
#&gt; GSM559388     4  0.6123      0.717 0.100 0.156 0.000 0.668 0.076
#&gt; GSM559389     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000      0.832 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559392     2  0.1270      0.922 0.000 0.948 0.000 0.000 0.052
#&gt; GSM559393     1  0.0609      0.931 0.980 0.000 0.000 0.000 0.020
#&gt; GSM559394     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559396     1  0.0162      0.940 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559398     2  0.0162      0.941 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559399     1  0.0162      0.940 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559400     4  0.2707      0.827 0.100 0.000 0.000 0.876 0.024
#&gt; GSM559402     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.0162      0.831 0.004 0.000 0.000 0.996 0.000
#&gt; GSM559407     1  0.1043      0.925 0.960 0.000 0.000 0.040 0.000
#&gt; GSM559408     1  0.2280      0.882 0.880 0.000 0.000 0.120 0.000
#&gt; GSM559409     1  0.2329      0.880 0.876 0.000 0.000 0.124 0.000
#&gt; GSM559410     1  0.2020      0.893 0.900 0.000 0.000 0.100 0.000
#&gt; GSM559411     1  0.2020      0.893 0.900 0.000 0.000 0.100 0.000
#&gt; GSM559412     1  0.2280      0.882 0.880 0.000 0.000 0.120 0.000
#&gt; GSM559413     1  0.2280      0.882 0.880 0.000 0.000 0.120 0.000
#&gt; GSM559415     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.0000      0.832 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559417     4  0.0000      0.832 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559418     1  0.0162      0.940 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559419     1  0.4278      0.240 0.548 0.000 0.000 0.452 0.000
#&gt; GSM559420     1  0.0162      0.940 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559421     2  0.1270      0.922 0.000 0.948 0.000 0.000 0.052
#&gt; GSM559423     2  0.1851      0.910 0.000 0.912 0.000 0.000 0.088
#&gt; GSM559425     2  0.0000      0.942 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.2690      0.855 0.000 0.844 0.000 0.000 0.156
#&gt; GSM559427     2  0.0000      0.942 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     4  0.5525      0.702 0.124 0.000 0.000 0.636 0.240
#&gt; GSM559429     2  0.3039      0.829 0.000 0.808 0.000 0.000 0.192
#&gt; GSM559430     2  0.0000      0.942 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559395     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559414     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.1610     1.0000 0.000 0.000 0.084 0.000 0.916 0.000
#&gt; GSM559424     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559431     2  0.0547     0.8849 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM559432     5  0.1610     1.0000 0.000 0.000 0.084 0.000 0.916 0.000
#&gt; GSM559381     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     4  0.5855     0.2364 0.072 0.080 0.000 0.616 0.004 0.228
#&gt; GSM559384     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559385     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559386     4  0.3315     0.5821 0.076 0.000 0.000 0.820 0.000 0.104
#&gt; GSM559388     6  0.6823     0.0976 0.064 0.156 0.000 0.384 0.004 0.392
#&gt; GSM559389     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000     0.7120 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559392     2  0.2964     0.7827 0.000 0.792 0.000 0.000 0.004 0.204
#&gt; GSM559393     1  0.2631     0.7720 0.820 0.000 0.000 0.000 0.000 0.180
#&gt; GSM559394     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559396     1  0.2389     0.8608 0.888 0.000 0.000 0.052 0.060 0.000
#&gt; GSM559398     2  0.1411     0.8677 0.000 0.936 0.000 0.000 0.004 0.060
#&gt; GSM559399     1  0.1957     0.8479 0.888 0.000 0.000 0.112 0.000 0.000
#&gt; GSM559400     4  0.4176     0.4789 0.064 0.000 0.000 0.716 0.000 0.220
#&gt; GSM559402     1  0.0937     0.8854 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; GSM559403     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000     0.8950 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.1957     0.5864 0.112 0.000 0.000 0.888 0.000 0.000
#&gt; GSM559407     1  0.0713     0.8901 0.972 0.000 0.000 0.028 0.000 0.000
#&gt; GSM559408     1  0.2562     0.8207 0.828 0.000 0.000 0.172 0.000 0.000
#&gt; GSM559409     1  0.2793     0.8113 0.800 0.000 0.000 0.200 0.000 0.000
#&gt; GSM559410     1  0.2048     0.8531 0.880 0.000 0.000 0.120 0.000 0.000
#&gt; GSM559411     1  0.2997     0.8489 0.844 0.000 0.000 0.096 0.060 0.000
#&gt; GSM559412     1  0.2562     0.8207 0.828 0.000 0.000 0.172 0.000 0.000
#&gt; GSM559413     1  0.3763     0.7865 0.768 0.000 0.000 0.172 0.060 0.000
#&gt; GSM559415     1  0.0146     0.8944 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559416     4  0.0000     0.7120 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559417     4  0.0146     0.7105 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559418     1  0.1957     0.8479 0.888 0.000 0.000 0.112 0.000 0.000
#&gt; GSM559419     1  0.4471     0.2798 0.500 0.000 0.000 0.472 0.028 0.000
#&gt; GSM559420     1  0.1957     0.8479 0.888 0.000 0.000 0.112 0.000 0.000
#&gt; GSM559421     2  0.2738     0.8042 0.000 0.820 0.000 0.000 0.004 0.176
#&gt; GSM559423     2  0.2838     0.8176 0.000 0.808 0.000 0.000 0.004 0.188
#&gt; GSM559425     2  0.0547     0.8849 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM559426     2  0.2454     0.7685 0.000 0.840 0.000 0.000 0.000 0.160
#&gt; GSM559427     2  0.0547     0.8849 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM559428     6  0.4972     0.2069 0.080 0.000 0.000 0.352 0.000 0.568
#&gt; GSM559429     6  0.2823     0.1437 0.000 0.204 0.000 0.000 0.000 0.796
#&gt; GSM559430     2  0.0000     0.8844 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:pam 52         1.99e-10 2
#> SD:pam 51         1.24e-10 3
#> SD:pam 51         6.63e-10 4
#> SD:pam 51         2.87e-09 5
#> SD:pam 46         2.54e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.981       0.987         0.3279 0.683   0.683
#> 3 3 0.687           0.819       0.911         0.8960 0.704   0.567
#> 4 4 0.703           0.562       0.820         0.1283 0.873   0.695
#> 5 5 0.707           0.772       0.808         0.0624 0.910   0.715
#> 6 6 0.715           0.707       0.818         0.0400 0.955   0.820
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.0000      1.000 0.000 1.000
#&gt; GSM559387     2  0.0000      1.000 0.000 1.000
#&gt; GSM559391     2  0.0000      1.000 0.000 1.000
#&gt; GSM559395     2  0.0000      1.000 0.000 1.000
#&gt; GSM559397     2  0.0000      1.000 0.000 1.000
#&gt; GSM559401     2  0.0000      1.000 0.000 1.000
#&gt; GSM559414     2  0.0000      1.000 0.000 1.000
#&gt; GSM559422     2  0.0000      1.000 0.000 1.000
#&gt; GSM559424     2  0.0000      1.000 0.000 1.000
#&gt; GSM559431     1  0.4815      0.908 0.896 0.104
#&gt; GSM559432     2  0.0000      1.000 0.000 1.000
#&gt; GSM559381     1  0.0000      0.984 1.000 0.000
#&gt; GSM559382     1  0.1843      0.976 0.972 0.028
#&gt; GSM559384     1  0.0000      0.984 1.000 0.000
#&gt; GSM559385     1  0.0000      0.984 1.000 0.000
#&gt; GSM559386     1  0.1184      0.980 0.984 0.016
#&gt; GSM559388     1  0.1843      0.976 0.972 0.028
#&gt; GSM559389     1  0.0000      0.984 1.000 0.000
#&gt; GSM559390     1  0.0000      0.984 1.000 0.000
#&gt; GSM559392     1  0.1843      0.976 0.972 0.028
#&gt; GSM559393     1  0.0000      0.984 1.000 0.000
#&gt; GSM559394     1  0.0000      0.984 1.000 0.000
#&gt; GSM559396     1  0.2236      0.970 0.964 0.036
#&gt; GSM559398     1  0.1843      0.976 0.972 0.028
#&gt; GSM559399     1  0.0000      0.984 1.000 0.000
#&gt; GSM559400     1  0.1843      0.976 0.972 0.028
#&gt; GSM559402     1  0.0000      0.984 1.000 0.000
#&gt; GSM559403     1  0.0000      0.984 1.000 0.000
#&gt; GSM559404     1  0.0000      0.984 1.000 0.000
#&gt; GSM559405     1  0.0000      0.984 1.000 0.000
#&gt; GSM559406     1  0.0000      0.984 1.000 0.000
#&gt; GSM559407     1  0.0000      0.984 1.000 0.000
#&gt; GSM559408     1  0.0000      0.984 1.000 0.000
#&gt; GSM559409     1  0.0000      0.984 1.000 0.000
#&gt; GSM559410     1  0.0000      0.984 1.000 0.000
#&gt; GSM559411     1  0.0000      0.984 1.000 0.000
#&gt; GSM559412     1  0.0000      0.984 1.000 0.000
#&gt; GSM559413     1  0.0000      0.984 1.000 0.000
#&gt; GSM559415     1  0.0000      0.984 1.000 0.000
#&gt; GSM559416     1  0.0000      0.984 1.000 0.000
#&gt; GSM559417     1  0.0000      0.984 1.000 0.000
#&gt; GSM559418     1  0.0672      0.982 0.992 0.008
#&gt; GSM559419     1  0.0000      0.984 1.000 0.000
#&gt; GSM559420     1  0.0000      0.984 1.000 0.000
#&gt; GSM559421     1  0.1843      0.976 0.972 0.028
#&gt; GSM559423     1  0.1843      0.976 0.972 0.028
#&gt; GSM559425     1  0.1843      0.976 0.972 0.028
#&gt; GSM559426     1  0.1843      0.976 0.972 0.028
#&gt; GSM559427     1  0.1843      0.976 0.972 0.028
#&gt; GSM559428     1  0.4815      0.908 0.896 0.104
#&gt; GSM559429     1  0.4815      0.908 0.896 0.104
#&gt; GSM559430     1  0.1843      0.976 0.972 0.028
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559432     3  0.1860      0.944 0.000 0.052 0.948
#&gt; GSM559381     1  0.1529      0.849 0.960 0.040 0.000
#&gt; GSM559382     2  0.0424      0.905 0.008 0.992 0.000
#&gt; GSM559384     1  0.2537      0.843 0.920 0.080 0.000
#&gt; GSM559385     1  0.5016      0.622 0.760 0.240 0.000
#&gt; GSM559386     2  0.6154      0.200 0.408 0.592 0.000
#&gt; GSM559388     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000      0.843 1.000 0.000 0.000
#&gt; GSM559390     1  0.4842      0.750 0.776 0.224 0.000
#&gt; GSM559392     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559393     1  0.5016      0.622 0.760 0.240 0.000
#&gt; GSM559394     1  0.5016      0.622 0.760 0.240 0.000
#&gt; GSM559396     1  0.6180      0.589 0.660 0.332 0.008
#&gt; GSM559398     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559399     1  0.2261      0.846 0.932 0.068 0.000
#&gt; GSM559400     1  0.6280      0.266 0.540 0.460 0.000
#&gt; GSM559402     1  0.0237      0.845 0.996 0.004 0.000
#&gt; GSM559403     1  0.0000      0.843 1.000 0.000 0.000
#&gt; GSM559404     1  0.2261      0.829 0.932 0.068 0.000
#&gt; GSM559405     1  0.0000      0.843 1.000 0.000 0.000
#&gt; GSM559406     1  0.4842      0.750 0.776 0.224 0.000
#&gt; GSM559407     1  0.0237      0.845 0.996 0.004 0.000
#&gt; GSM559408     1  0.0592      0.846 0.988 0.012 0.000
#&gt; GSM559409     1  0.1643      0.850 0.956 0.044 0.000
#&gt; GSM559410     1  0.0000      0.843 1.000 0.000 0.000
#&gt; GSM559411     1  0.4842      0.750 0.776 0.224 0.000
#&gt; GSM559412     1  0.3116      0.833 0.892 0.108 0.000
#&gt; GSM559413     1  0.4842      0.750 0.776 0.224 0.000
#&gt; GSM559415     1  0.0000      0.843 1.000 0.000 0.000
#&gt; GSM559416     1  0.0747      0.847 0.984 0.016 0.000
#&gt; GSM559417     1  0.2796      0.840 0.908 0.092 0.000
#&gt; GSM559418     1  0.6180      0.376 0.584 0.416 0.000
#&gt; GSM559419     1  0.2261      0.847 0.932 0.068 0.000
#&gt; GSM559420     1  0.4452      0.778 0.808 0.192 0.000
#&gt; GSM559421     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.910 0.000 1.000 0.000
#&gt; GSM559428     2  0.5016      0.626 0.240 0.760 0.000
#&gt; GSM559429     2  0.5016      0.626 0.240 0.760 0.000
#&gt; GSM559430     2  0.0000      0.910 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.3610     0.8603 0.000 0.000 0.800 0.200
#&gt; GSM559414     3  0.0000     0.9252 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.4454     0.8099 0.000 0.000 0.692 0.308
#&gt; GSM559424     3  0.2053     0.8779 0.072 0.000 0.924 0.004
#&gt; GSM559431     2  0.0188     0.9558 0.004 0.996 0.000 0.000
#&gt; GSM559432     3  0.4855     0.7529 0.000 0.000 0.600 0.400
#&gt; GSM559381     1  0.4981    -0.1815 0.536 0.000 0.000 0.464
#&gt; GSM559382     2  0.0817     0.9444 0.024 0.976 0.000 0.000
#&gt; GSM559384     1  0.4866     0.0398 0.596 0.000 0.000 0.404
#&gt; GSM559385     4  0.4477     0.9014 0.312 0.000 0.000 0.688
#&gt; GSM559386     1  0.7654    -0.2054 0.464 0.252 0.000 0.284
#&gt; GSM559388     2  0.0188     0.9553 0.000 0.996 0.000 0.004
#&gt; GSM559389     1  0.4992    -0.2295 0.524 0.000 0.000 0.476
#&gt; GSM559390     1  0.1305     0.4545 0.960 0.036 0.000 0.004
#&gt; GSM559392     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559393     4  0.4477     0.9014 0.312 0.000 0.000 0.688
#&gt; GSM559394     4  0.4477     0.9014 0.312 0.000 0.000 0.688
#&gt; GSM559396     1  0.3765     0.2958 0.812 0.180 0.004 0.004
#&gt; GSM559398     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.4624     0.1768 0.660 0.000 0.000 0.340
#&gt; GSM559400     2  0.5088     0.4712 0.424 0.572 0.000 0.004
#&gt; GSM559402     1  0.4624     0.1783 0.660 0.000 0.000 0.340
#&gt; GSM559403     1  0.5000    -0.3227 0.504 0.000 0.000 0.496
#&gt; GSM559404     4  0.4877     0.6389 0.408 0.000 0.000 0.592
#&gt; GSM559405     1  0.4992    -0.2295 0.524 0.000 0.000 0.476
#&gt; GSM559406     1  0.1305     0.4545 0.960 0.036 0.000 0.004
#&gt; GSM559407     1  0.4564     0.2043 0.672 0.000 0.000 0.328
#&gt; GSM559408     1  0.2704     0.4453 0.876 0.000 0.000 0.124
#&gt; GSM559409     1  0.3764     0.3834 0.784 0.000 0.000 0.216
#&gt; GSM559410     1  0.4661     0.1535 0.652 0.000 0.000 0.348
#&gt; GSM559411     1  0.1305     0.4567 0.960 0.036 0.000 0.004
#&gt; GSM559412     1  0.1211     0.4682 0.960 0.000 0.000 0.040
#&gt; GSM559413     1  0.0469     0.4634 0.988 0.000 0.000 0.012
#&gt; GSM559415     1  0.4981    -0.2024 0.536 0.000 0.000 0.464
#&gt; GSM559416     1  0.3243     0.4390 0.876 0.036 0.000 0.088
#&gt; GSM559417     1  0.3243     0.4390 0.876 0.036 0.000 0.088
#&gt; GSM559418     1  0.6709    -0.2617 0.508 0.092 0.000 0.400
#&gt; GSM559419     1  0.3731     0.4387 0.844 0.036 0.000 0.120
#&gt; GSM559420     1  0.4546     0.3284 0.732 0.012 0.000 0.256
#&gt; GSM559421     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000     0.9573 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.1792     0.9075 0.068 0.932 0.000 0.000
#&gt; GSM559429     2  0.0469     0.9522 0.012 0.988 0.000 0.000
#&gt; GSM559430     2  0.0000     0.9573 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000      0.880 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000      0.880 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0404      0.877 0.000 0.000 0.988 0.012 0.000
#&gt; GSM559395     3  0.0000      0.880 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000      0.880 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.5484      0.716 0.000 0.000 0.640 0.240 0.120
#&gt; GSM559414     3  0.0000      0.880 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.5888      0.678 0.000 0.000 0.576 0.288 0.136
#&gt; GSM559424     3  0.1043      0.865 0.000 0.000 0.960 0.040 0.000
#&gt; GSM559431     2  0.1671      0.882 0.000 0.924 0.000 0.000 0.076
#&gt; GSM559432     3  0.6309      0.628 0.000 0.000 0.520 0.288 0.192
#&gt; GSM559381     1  0.0324      0.815 0.992 0.004 0.000 0.000 0.004
#&gt; GSM559382     2  0.3130      0.785 0.096 0.856 0.000 0.048 0.000
#&gt; GSM559384     1  0.0566      0.812 0.984 0.000 0.000 0.012 0.004
#&gt; GSM559385     5  0.4114      0.968 0.376 0.000 0.000 0.000 0.624
#&gt; GSM559386     1  0.3132      0.587 0.820 0.172 0.000 0.000 0.008
#&gt; GSM559388     2  0.0510      0.886 0.016 0.984 0.000 0.000 0.000
#&gt; GSM559389     1  0.0451      0.811 0.988 0.004 0.000 0.000 0.008
#&gt; GSM559390     4  0.5971      0.804 0.396 0.000 0.000 0.492 0.112
#&gt; GSM559392     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559393     5  0.4251      0.964 0.372 0.004 0.000 0.000 0.624
#&gt; GSM559394     5  0.4114      0.968 0.376 0.000 0.000 0.000 0.624
#&gt; GSM559396     4  0.5161      0.697 0.260 0.044 0.000 0.676 0.020
#&gt; GSM559398     2  0.1410      0.884 0.000 0.940 0.000 0.000 0.060
#&gt; GSM559399     1  0.0451      0.814 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559400     2  0.6230      0.328 0.008 0.480 0.000 0.400 0.112
#&gt; GSM559402     1  0.1195      0.807 0.960 0.000 0.000 0.012 0.028
#&gt; GSM559403     1  0.1792      0.720 0.916 0.000 0.000 0.000 0.084
#&gt; GSM559404     5  0.4557      0.905 0.404 0.000 0.000 0.012 0.584
#&gt; GSM559405     1  0.0162      0.813 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559406     4  0.4138      0.812 0.384 0.000 0.000 0.616 0.000
#&gt; GSM559407     1  0.1195      0.807 0.960 0.000 0.000 0.012 0.028
#&gt; GSM559408     1  0.3695      0.597 0.800 0.000 0.000 0.164 0.036
#&gt; GSM559409     1  0.3772      0.597 0.792 0.000 0.000 0.172 0.036
#&gt; GSM559410     1  0.0451      0.814 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559411     4  0.4242      0.796 0.428 0.000 0.000 0.572 0.000
#&gt; GSM559412     1  0.3724      0.535 0.776 0.000 0.000 0.204 0.020
#&gt; GSM559413     1  0.4390     -0.457 0.568 0.000 0.000 0.428 0.004
#&gt; GSM559415     1  0.0486      0.814 0.988 0.004 0.000 0.004 0.004
#&gt; GSM559416     4  0.5338      0.839 0.400 0.000 0.000 0.544 0.056
#&gt; GSM559417     4  0.5345      0.839 0.404 0.000 0.000 0.540 0.056
#&gt; GSM559418     1  0.0992      0.799 0.968 0.024 0.000 0.000 0.008
#&gt; GSM559419     4  0.5106      0.729 0.456 0.000 0.000 0.508 0.036
#&gt; GSM559420     1  0.3922      0.569 0.780 0.000 0.000 0.180 0.040
#&gt; GSM559421     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.1410      0.884 0.000 0.940 0.000 0.000 0.060
#&gt; GSM559426     2  0.0162      0.889 0.004 0.996 0.000 0.000 0.000
#&gt; GSM559427     2  0.1410      0.884 0.000 0.940 0.000 0.000 0.060
#&gt; GSM559428     2  0.4735      0.639 0.012 0.668 0.000 0.300 0.020
#&gt; GSM559429     2  0.3357      0.811 0.012 0.836 0.000 0.136 0.016
#&gt; GSM559430     2  0.1270      0.886 0.000 0.948 0.000 0.000 0.052
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0547   0.836783 0.000 0.000 0.980 0.000 0.020 0.000
#&gt; GSM559387     3  0.0000   0.843048 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.3023   0.748649 0.000 0.000 0.828 0.140 0.032 0.000
#&gt; GSM559395     3  0.0363   0.836157 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559397     3  0.0000   0.843048 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.3797  -0.222702 0.000 0.000 0.580 0.000 0.420 0.000
#&gt; GSM559414     3  0.0000   0.843048 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.3288   1.000000 0.000 0.000 0.276 0.000 0.724 0.000
#&gt; GSM559424     3  0.3023   0.748649 0.000 0.000 0.828 0.140 0.032 0.000
#&gt; GSM559431     2  0.4462   0.797170 0.000 0.712 0.000 0.152 0.136 0.000
#&gt; GSM559432     5  0.3288   1.000000 0.000 0.000 0.276 0.000 0.724 0.000
#&gt; GSM559381     1  0.0000   0.742473 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     2  0.1082   0.868721 0.000 0.956 0.000 0.040 0.000 0.004
#&gt; GSM559384     1  0.1644   0.736078 0.932 0.000 0.000 0.000 0.040 0.028
#&gt; GSM559385     6  0.2491   0.984436 0.164 0.000 0.000 0.000 0.000 0.836
#&gt; GSM559386     1  0.2320   0.622842 0.864 0.132 0.000 0.000 0.000 0.004
#&gt; GSM559388     2  0.0000   0.878280 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559389     1  0.0363   0.742446 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM559390     4  0.5510   0.732402 0.292 0.000 0.000 0.584 0.020 0.104
#&gt; GSM559392     2  0.0000   0.878280 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559393     6  0.2664   0.975428 0.184 0.000 0.000 0.000 0.000 0.816
#&gt; GSM559394     6  0.2562   0.985542 0.172 0.000 0.000 0.000 0.000 0.828
#&gt; GSM559396     4  0.3699   0.624829 0.160 0.000 0.000 0.788 0.040 0.012
#&gt; GSM559398     2  0.2300   0.851415 0.000 0.856 0.000 0.144 0.000 0.000
#&gt; GSM559399     1  0.0260   0.742631 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM559400     2  0.5990   0.463123 0.000 0.576 0.000 0.252 0.052 0.120
#&gt; GSM559402     1  0.0551   0.741108 0.984 0.000 0.000 0.004 0.004 0.008
#&gt; GSM559403     1  0.2416   0.610173 0.844 0.000 0.000 0.000 0.000 0.156
#&gt; GSM559404     6  0.2527   0.983803 0.168 0.000 0.000 0.000 0.000 0.832
#&gt; GSM559405     1  0.0547   0.741421 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM559406     1  0.4468   0.000782 0.560 0.000 0.000 0.408 0.032 0.000
#&gt; GSM559407     1  0.1622   0.727065 0.940 0.000 0.000 0.028 0.016 0.016
#&gt; GSM559408     1  0.5073   0.502179 0.692 0.000 0.000 0.180 0.084 0.044
#&gt; GSM559409     1  0.4915   0.530966 0.712 0.000 0.000 0.160 0.084 0.044
#&gt; GSM559410     1  0.0632   0.740726 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM559411     4  0.4532   0.270860 0.468 0.000 0.000 0.500 0.032 0.000
#&gt; GSM559412     1  0.5330   0.414606 0.648 0.000 0.000 0.228 0.084 0.040
#&gt; GSM559413     1  0.4709   0.178045 0.596 0.000 0.000 0.352 0.048 0.004
#&gt; GSM559415     1  0.0632   0.740726 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM559416     4  0.4096   0.751258 0.304 0.000 0.000 0.672 0.008 0.016
#&gt; GSM559417     4  0.4717   0.759303 0.308 0.000 0.000 0.632 0.008 0.052
#&gt; GSM559418     1  0.0993   0.735942 0.964 0.012 0.000 0.000 0.000 0.024
#&gt; GSM559419     1  0.5535  -0.158291 0.516 0.000 0.000 0.392 0.048 0.044
#&gt; GSM559420     1  0.4739   0.553571 0.732 0.000 0.000 0.140 0.084 0.044
#&gt; GSM559421     2  0.0000   0.878280 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0000   0.878280 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.2553   0.848100 0.000 0.848 0.000 0.144 0.008 0.000
#&gt; GSM559426     2  0.0820   0.871008 0.016 0.972 0.000 0.000 0.000 0.012
#&gt; GSM559427     2  0.2553   0.848100 0.000 0.848 0.000 0.144 0.008 0.000
#&gt; GSM559428     2  0.2890   0.822456 0.004 0.844 0.000 0.024 0.128 0.000
#&gt; GSM559429     2  0.2667   0.826717 0.000 0.852 0.000 0.020 0.128 0.000
#&gt; GSM559430     2  0.2300   0.851415 0.000 0.856 0.000 0.144 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:mclust 52         1.99e-10 2
#> SD:mclust 49         3.24e-10 3
#> SD:mclust 27         9.27e-06 4
#> SD:mclust 50         4.77e-09 5
#> SD:mclust 45         1.75e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.422           0.782       0.858         0.4516 0.527   0.527
#> 3 3 0.998           0.948       0.978         0.3760 0.687   0.490
#> 4 4 0.730           0.769       0.885         0.1802 0.855   0.638
#> 5 5 0.700           0.638       0.793         0.0693 0.922   0.729
#> 6 6 0.716           0.631       0.786         0.0509 0.847   0.451
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.000      0.752 1.000 0.000
#&gt; GSM559387     1   0.000      0.752 1.000 0.000
#&gt; GSM559391     1   0.000      0.752 1.000 0.000
#&gt; GSM559395     1   0.000      0.752 1.000 0.000
#&gt; GSM559397     1   0.000      0.752 1.000 0.000
#&gt; GSM559401     1   0.000      0.752 1.000 0.000
#&gt; GSM559414     1   0.000      0.752 1.000 0.000
#&gt; GSM559422     1   0.000      0.752 1.000 0.000
#&gt; GSM559424     1   0.000      0.752 1.000 0.000
#&gt; GSM559431     2   0.327      0.883 0.060 0.940
#&gt; GSM559432     1   0.999     -0.205 0.516 0.484
#&gt; GSM559381     1   0.999      0.459 0.516 0.484
#&gt; GSM559382     2   0.000      0.947 0.000 1.000
#&gt; GSM559384     1   0.738      0.832 0.792 0.208
#&gt; GSM559385     1   0.760      0.828 0.780 0.220
#&gt; GSM559386     2   0.000      0.947 0.000 1.000
#&gt; GSM559388     2   0.000      0.947 0.000 1.000
#&gt; GSM559389     1   0.981      0.602 0.580 0.420
#&gt; GSM559390     1   0.808      0.813 0.752 0.248
#&gt; GSM559392     2   0.000      0.947 0.000 1.000
#&gt; GSM559393     2   0.118      0.931 0.016 0.984
#&gt; GSM559394     1   0.971      0.637 0.600 0.400
#&gt; GSM559396     1   0.697      0.829 0.812 0.188
#&gt; GSM559398     2   0.000      0.947 0.000 1.000
#&gt; GSM559399     1   0.997      0.500 0.532 0.468
#&gt; GSM559400     2   0.000      0.947 0.000 1.000
#&gt; GSM559402     1   0.909      0.747 0.676 0.324
#&gt; GSM559403     1   0.855      0.790 0.720 0.280
#&gt; GSM559404     1   0.697      0.829 0.812 0.188
#&gt; GSM559405     1   0.760      0.828 0.780 0.220
#&gt; GSM559406     1   0.730      0.831 0.796 0.204
#&gt; GSM559407     1   0.753      0.830 0.784 0.216
#&gt; GSM559408     1   0.738      0.832 0.792 0.208
#&gt; GSM559409     1   0.738      0.832 0.792 0.208
#&gt; GSM559410     1   0.876      0.776 0.704 0.296
#&gt; GSM559411     1   0.738      0.832 0.792 0.208
#&gt; GSM559412     1   0.738      0.832 0.792 0.208
#&gt; GSM559413     1   0.730      0.831 0.796 0.204
#&gt; GSM559415     2   0.992     -0.305 0.448 0.552
#&gt; GSM559416     1   0.921      0.737 0.664 0.336
#&gt; GSM559417     2   0.402      0.847 0.080 0.920
#&gt; GSM559418     2   0.000      0.947 0.000 1.000
#&gt; GSM559419     1   0.861      0.788 0.716 0.284
#&gt; GSM559420     1   0.745      0.831 0.788 0.212
#&gt; GSM559421     2   0.000      0.947 0.000 1.000
#&gt; GSM559423     2   0.000      0.947 0.000 1.000
#&gt; GSM559425     2   0.000      0.947 0.000 1.000
#&gt; GSM559426     2   0.000      0.947 0.000 1.000
#&gt; GSM559427     2   0.000      0.947 0.000 1.000
#&gt; GSM559428     2   0.388      0.863 0.076 0.924
#&gt; GSM559429     2   0.000      0.947 0.000 1.000
#&gt; GSM559430     2   0.000      0.947 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559387     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559391     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559395     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559397     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559401     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559414     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559422     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559424     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559431     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559432     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM559381     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559382     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559384     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559385     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559386     1   0.522      0.664 0.740 0.260 0.000
#&gt; GSM559388     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559389     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559390     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559392     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559393     1   0.382      0.821 0.852 0.148 0.000
#&gt; GSM559394     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559396     1   0.288      0.875 0.904 0.000 0.096
#&gt; GSM559398     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559399     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559400     2   0.210      0.927 0.052 0.944 0.004
#&gt; GSM559402     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559403     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559404     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559405     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559406     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559407     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559408     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559409     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559410     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559411     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559412     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559413     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559415     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559416     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559417     1   0.153      0.929 0.960 0.040 0.000
#&gt; GSM559418     1   0.630      0.154 0.528 0.472 0.000
#&gt; GSM559419     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559420     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM559421     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559423     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559425     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559426     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559427     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559428     2   0.175      0.947 0.000 0.952 0.048
#&gt; GSM559429     2   0.000      0.991 0.000 1.000 0.000
#&gt; GSM559430     2   0.000      0.991 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.4730     0.4682 0.000 0.000 0.636 0.364
#&gt; GSM559387     3  0.1792     0.9102 0.000 0.000 0.932 0.068
#&gt; GSM559391     4  0.4713     0.2554 0.000 0.000 0.360 0.640
#&gt; GSM559395     3  0.1302     0.9196 0.000 0.000 0.956 0.044
#&gt; GSM559397     3  0.1557     0.9163 0.000 0.000 0.944 0.056
#&gt; GSM559401     3  0.0000     0.9131 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.1302     0.9197 0.000 0.000 0.956 0.044
#&gt; GSM559422     3  0.0592     0.9061 0.000 0.000 0.984 0.016
#&gt; GSM559424     4  0.4304     0.4330 0.000 0.000 0.284 0.716
#&gt; GSM559431     2  0.0817     0.9365 0.000 0.976 0.000 0.024
#&gt; GSM559432     3  0.0336     0.9103 0.000 0.000 0.992 0.008
#&gt; GSM559381     1  0.0336     0.8559 0.992 0.000 0.000 0.008
#&gt; GSM559382     2  0.2342     0.8916 0.008 0.912 0.000 0.080
#&gt; GSM559384     1  0.1940     0.8240 0.924 0.000 0.000 0.076
#&gt; GSM559385     1  0.0469     0.8554 0.988 0.000 0.000 0.012
#&gt; GSM559386     1  0.5894     0.3452 0.568 0.392 0.000 0.040
#&gt; GSM559388     2  0.1151     0.9298 0.008 0.968 0.000 0.024
#&gt; GSM559389     1  0.0336     0.8549 0.992 0.000 0.000 0.008
#&gt; GSM559390     4  0.2466     0.7734 0.096 0.000 0.004 0.900
#&gt; GSM559392     2  0.0707     0.9338 0.000 0.980 0.000 0.020
#&gt; GSM559393     1  0.2845     0.7992 0.896 0.076 0.000 0.028
#&gt; GSM559394     1  0.0895     0.8504 0.976 0.004 0.000 0.020
#&gt; GSM559396     4  0.3820     0.6937 0.100 0.016 0.028 0.856
#&gt; GSM559398     2  0.0707     0.9338 0.000 0.980 0.000 0.020
#&gt; GSM559399     1  0.0188     0.8571 0.996 0.000 0.000 0.004
#&gt; GSM559400     2  0.5214     0.4514 0.008 0.624 0.004 0.364
#&gt; GSM559402     1  0.1474     0.8446 0.948 0.000 0.000 0.052
#&gt; GSM559403     1  0.0188     0.8571 0.996 0.000 0.000 0.004
#&gt; GSM559404     1  0.0188     0.8571 0.996 0.000 0.000 0.004
#&gt; GSM559405     1  0.0188     0.8571 0.996 0.000 0.000 0.004
#&gt; GSM559406     4  0.3311     0.7515 0.172 0.000 0.000 0.828
#&gt; GSM559407     1  0.2469     0.8008 0.892 0.000 0.000 0.108
#&gt; GSM559408     1  0.4406     0.5468 0.700 0.000 0.000 0.300
#&gt; GSM559409     1  0.2760     0.7828 0.872 0.000 0.000 0.128
#&gt; GSM559410     1  0.0188     0.8571 0.996 0.000 0.000 0.004
#&gt; GSM559411     4  0.2530     0.7763 0.112 0.000 0.000 0.888
#&gt; GSM559412     1  0.4804     0.3479 0.616 0.000 0.000 0.384
#&gt; GSM559413     4  0.4977     0.1198 0.460 0.000 0.000 0.540
#&gt; GSM559415     1  0.0336     0.8549 0.992 0.000 0.000 0.008
#&gt; GSM559416     4  0.2741     0.7742 0.096 0.012 0.000 0.892
#&gt; GSM559417     4  0.5417     0.6582 0.088 0.180 0.000 0.732
#&gt; GSM559418     1  0.3695     0.7314 0.828 0.156 0.000 0.016
#&gt; GSM559419     4  0.3123     0.7642 0.156 0.000 0.000 0.844
#&gt; GSM559420     1  0.4961     0.0751 0.552 0.000 0.000 0.448
#&gt; GSM559421     2  0.1576     0.9240 0.004 0.948 0.000 0.048
#&gt; GSM559423     2  0.1209     0.9341 0.004 0.964 0.000 0.032
#&gt; GSM559425     2  0.0592     0.9373 0.000 0.984 0.000 0.016
#&gt; GSM559426     2  0.1398     0.9318 0.004 0.956 0.000 0.040
#&gt; GSM559427     2  0.0336     0.9357 0.000 0.992 0.000 0.008
#&gt; GSM559428     2  0.3292     0.8785 0.004 0.880 0.080 0.036
#&gt; GSM559429     2  0.1398     0.9318 0.004 0.956 0.000 0.040
#&gt; GSM559430     2  0.0657     0.9375 0.004 0.984 0.000 0.012
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     5  0.5953   -0.18051 0.000 0.000 0.384 0.112 0.504
#&gt; GSM559387     3  0.4617    0.52827 0.000 0.000 0.552 0.012 0.436
#&gt; GSM559391     5  0.5756    0.55620 0.000 0.000 0.112 0.312 0.576
#&gt; GSM559395     3  0.4582    0.56805 0.000 0.000 0.572 0.012 0.416
#&gt; GSM559397     3  0.4425    0.59967 0.000 0.000 0.600 0.008 0.392
#&gt; GSM559401     3  0.2074    0.59567 0.000 0.000 0.896 0.000 0.104
#&gt; GSM559414     3  0.4436    0.59732 0.000 0.000 0.596 0.008 0.396
#&gt; GSM559422     3  0.0000    0.55617 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559424     5  0.5498    0.55841 0.000 0.000 0.080 0.340 0.580
#&gt; GSM559431     2  0.3845    0.78982 0.000 0.760 0.012 0.004 0.224
#&gt; GSM559432     3  0.0162    0.55923 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559381     1  0.1469    0.79731 0.948 0.000 0.000 0.016 0.036
#&gt; GSM559382     2  0.3852    0.60294 0.000 0.760 0.000 0.220 0.020
#&gt; GSM559384     1  0.4206    0.63305 0.708 0.000 0.000 0.020 0.272
#&gt; GSM559385     1  0.0000    0.80366 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559386     1  0.6105    0.25413 0.512 0.368 0.000 0.116 0.004
#&gt; GSM559388     2  0.2824    0.75240 0.008 0.880 0.000 0.088 0.024
#&gt; GSM559389     1  0.0451    0.80378 0.988 0.000 0.000 0.004 0.008
#&gt; GSM559390     4  0.1179    0.73371 0.016 0.004 0.000 0.964 0.016
#&gt; GSM559392     2  0.0898    0.80764 0.000 0.972 0.000 0.008 0.020
#&gt; GSM559393     1  0.1130    0.79865 0.968 0.004 0.004 0.012 0.012
#&gt; GSM559394     1  0.0404    0.80298 0.988 0.000 0.000 0.000 0.012
#&gt; GSM559396     5  0.2775    0.33289 0.036 0.008 0.000 0.068 0.888
#&gt; GSM559398     2  0.0992    0.80622 0.000 0.968 0.000 0.008 0.024
#&gt; GSM559399     1  0.0566    0.80234 0.984 0.004 0.000 0.000 0.012
#&gt; GSM559400     4  0.5319    0.40531 0.000 0.360 0.008 0.588 0.044
#&gt; GSM559402     1  0.5074    0.61653 0.700 0.000 0.000 0.168 0.132
#&gt; GSM559403     1  0.0000    0.80366 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0880    0.79676 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559405     1  0.0162    0.80369 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559406     4  0.1484    0.74695 0.048 0.000 0.000 0.944 0.008
#&gt; GSM559407     1  0.5309    0.47662 0.644 0.000 0.000 0.264 0.092
#&gt; GSM559408     4  0.3561    0.63257 0.260 0.000 0.000 0.740 0.000
#&gt; GSM559409     1  0.4440    0.00967 0.528 0.000 0.000 0.468 0.004
#&gt; GSM559410     1  0.1282    0.79187 0.952 0.000 0.000 0.044 0.004
#&gt; GSM559411     4  0.4029    0.52624 0.024 0.000 0.000 0.744 0.232
#&gt; GSM559412     4  0.4575    0.66254 0.236 0.000 0.000 0.712 0.052
#&gt; GSM559413     4  0.5137    0.65732 0.208 0.000 0.000 0.684 0.108
#&gt; GSM559415     1  0.1041    0.80073 0.964 0.000 0.000 0.004 0.032
#&gt; GSM559416     4  0.1095    0.73346 0.008 0.012 0.000 0.968 0.012
#&gt; GSM559417     4  0.2959    0.70077 0.008 0.112 0.000 0.864 0.016
#&gt; GSM559418     1  0.4914    0.44972 0.628 0.336 0.000 0.004 0.032
#&gt; GSM559419     4  0.2153    0.73538 0.040 0.000 0.000 0.916 0.044
#&gt; GSM559420     1  0.6674    0.19578 0.436 0.000 0.000 0.260 0.304
#&gt; GSM559421     2  0.1106    0.81897 0.000 0.964 0.000 0.012 0.024
#&gt; GSM559423     2  0.4152    0.76163 0.000 0.692 0.000 0.012 0.296
#&gt; GSM559425     2  0.2377    0.81861 0.000 0.872 0.000 0.000 0.128
#&gt; GSM559426     2  0.4108    0.75516 0.000 0.684 0.000 0.008 0.308
#&gt; GSM559427     2  0.0566    0.81747 0.000 0.984 0.000 0.004 0.012
#&gt; GSM559428     2  0.6359    0.62822 0.000 0.532 0.152 0.008 0.308
#&gt; GSM559429     2  0.4165    0.74780 0.000 0.672 0.000 0.008 0.320
#&gt; GSM559430     2  0.1732    0.82389 0.000 0.920 0.000 0.000 0.080
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.1082     0.8372 0.000 0.000 0.956 0.004 0.040 0.000
#&gt; GSM559387     3  0.2260     0.8532 0.000 0.000 0.860 0.000 0.140 0.000
#&gt; GSM559391     3  0.0935     0.7977 0.000 0.000 0.964 0.032 0.004 0.000
#&gt; GSM559395     3  0.2340     0.8508 0.000 0.000 0.852 0.000 0.148 0.000
#&gt; GSM559397     3  0.2762     0.8171 0.000 0.000 0.804 0.000 0.196 0.000
#&gt; GSM559401     5  0.3175     0.7193 0.000 0.000 0.256 0.000 0.744 0.000
#&gt; GSM559414     3  0.2854     0.8040 0.000 0.000 0.792 0.000 0.208 0.000
#&gt; GSM559422     5  0.1714     0.8806 0.000 0.000 0.092 0.000 0.908 0.000
#&gt; GSM559424     3  0.0935     0.7980 0.000 0.000 0.964 0.032 0.004 0.000
#&gt; GSM559431     6  0.4853     0.1414 0.000 0.456 0.000 0.000 0.056 0.488
#&gt; GSM559432     5  0.1765     0.8826 0.000 0.000 0.096 0.000 0.904 0.000
#&gt; GSM559381     1  0.3698     0.7749 0.788 0.000 0.000 0.028 0.020 0.164
#&gt; GSM559382     2  0.4768     0.5343 0.040 0.728 0.000 0.164 0.004 0.064
#&gt; GSM559384     1  0.5691     0.5154 0.564 0.000 0.032 0.028 0.036 0.340
#&gt; GSM559385     1  0.0862     0.8294 0.972 0.004 0.000 0.000 0.008 0.016
#&gt; GSM559386     1  0.6542     0.2830 0.504 0.312 0.000 0.104 0.008 0.072
#&gt; GSM559388     2  0.2699     0.6050 0.068 0.880 0.000 0.020 0.000 0.032
#&gt; GSM559389     1  0.1667     0.8296 0.936 0.004 0.000 0.008 0.008 0.044
#&gt; GSM559390     4  0.1830     0.7744 0.004 0.016 0.016 0.936 0.004 0.024
#&gt; GSM559392     2  0.0603     0.6428 0.000 0.980 0.000 0.000 0.004 0.016
#&gt; GSM559393     1  0.2716     0.7861 0.868 0.096 0.000 0.000 0.008 0.028
#&gt; GSM559394     1  0.1908     0.8176 0.924 0.044 0.000 0.000 0.012 0.020
#&gt; GSM559396     6  0.5377    -0.1154 0.004 0.000 0.460 0.028 0.040 0.468
#&gt; GSM559398     2  0.0291     0.6426 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM559399     1  0.2019     0.8289 0.924 0.032 0.000 0.004 0.020 0.020
#&gt; GSM559400     2  0.4537     0.1104 0.000 0.488 0.000 0.484 0.004 0.024
#&gt; GSM559402     4  0.7086     0.2198 0.276 0.008 0.008 0.384 0.032 0.292
#&gt; GSM559403     1  0.0405     0.8310 0.988 0.008 0.000 0.000 0.004 0.000
#&gt; GSM559404     1  0.3243     0.8027 0.844 0.000 0.000 0.064 0.016 0.076
#&gt; GSM559405     1  0.1914     0.8302 0.920 0.000 0.000 0.016 0.008 0.056
#&gt; GSM559406     4  0.1381     0.7841 0.020 0.004 0.000 0.952 0.004 0.020
#&gt; GSM559407     4  0.6659     0.5581 0.172 0.008 0.016 0.552 0.040 0.212
#&gt; GSM559408     4  0.1854     0.7962 0.020 0.004 0.000 0.932 0.016 0.028
#&gt; GSM559409     4  0.4543     0.6117 0.256 0.000 0.004 0.688 0.016 0.036
#&gt; GSM559410     1  0.2763     0.8079 0.868 0.000 0.000 0.088 0.008 0.036
#&gt; GSM559411     4  0.5354     0.7066 0.012 0.004 0.092 0.692 0.032 0.168
#&gt; GSM559412     4  0.2556     0.7879 0.008 0.000 0.000 0.864 0.008 0.120
#&gt; GSM559413     4  0.3526     0.7705 0.008 0.000 0.004 0.808 0.036 0.144
#&gt; GSM559415     1  0.3995     0.7401 0.768 0.000 0.000 0.032 0.028 0.172
#&gt; GSM559416     4  0.0914     0.7822 0.000 0.016 0.016 0.968 0.000 0.000
#&gt; GSM559417     4  0.1059     0.7866 0.000 0.016 0.000 0.964 0.016 0.004
#&gt; GSM559418     2  0.6200     0.0894 0.376 0.488 0.000 0.024 0.024 0.088
#&gt; GSM559419     4  0.2238     0.7976 0.004 0.004 0.016 0.900 0.000 0.076
#&gt; GSM559420     6  0.8152    -0.1809 0.248 0.008 0.128 0.224 0.032 0.360
#&gt; GSM559421     2  0.2778     0.5889 0.000 0.824 0.000 0.000 0.008 0.168
#&gt; GSM559423     6  0.3935     0.4447 0.004 0.292 0.000 0.000 0.016 0.688
#&gt; GSM559425     2  0.3288     0.4151 0.000 0.724 0.000 0.000 0.000 0.276
#&gt; GSM559426     6  0.3468     0.4892 0.004 0.284 0.000 0.000 0.000 0.712
#&gt; GSM559427     2  0.2135     0.6029 0.000 0.872 0.000 0.000 0.000 0.128
#&gt; GSM559428     6  0.4830     0.4743 0.000 0.172 0.000 0.000 0.160 0.668
#&gt; GSM559429     6  0.3652     0.5014 0.000 0.264 0.000 0.000 0.016 0.720
#&gt; GSM559430     2  0.3161     0.5085 0.000 0.776 0.000 0.000 0.008 0.216
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:NMF 49         1.10e-01 2
#> SD:NMF 51         1.31e-10 3
#> SD:NMF 44         3.12e-08 4
#> SD:NMF 44         7.87e-08 5
#> SD:NMF 41         9.38e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.409           0.807       0.871         0.3753 0.618   0.618
#> 3 3 0.662           0.830       0.897         0.4484 0.781   0.646
#> 4 4 0.530           0.787       0.828         0.1909 0.957   0.895
#> 5 5 0.658           0.654       0.807         0.1100 0.969   0.918
#> 6 6 0.643           0.445       0.776         0.0479 0.947   0.852
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.6973      0.766 0.812 0.188
#&gt; GSM559387     1  0.6973      0.766 0.812 0.188
#&gt; GSM559391     1  0.6973      0.766 0.812 0.188
#&gt; GSM559395     1  0.6973      0.766 0.812 0.188
#&gt; GSM559397     1  0.6973      0.766 0.812 0.188
#&gt; GSM559401     1  0.6973      0.766 0.812 0.188
#&gt; GSM559414     1  0.6973      0.766 0.812 0.188
#&gt; GSM559422     1  0.8763      0.676 0.704 0.296
#&gt; GSM559424     1  0.6973      0.766 0.812 0.188
#&gt; GSM559431     2  0.6973      0.904 0.188 0.812
#&gt; GSM559432     1  0.8763      0.676 0.704 0.296
#&gt; GSM559381     1  0.2948      0.849 0.948 0.052
#&gt; GSM559382     1  0.9815     -0.190 0.580 0.420
#&gt; GSM559384     1  0.0000      0.882 1.000 0.000
#&gt; GSM559385     1  0.0000      0.882 1.000 0.000
#&gt; GSM559386     1  0.9000      0.279 0.684 0.316
#&gt; GSM559388     2  0.9460      0.745 0.364 0.636
#&gt; GSM559389     1  0.2948      0.849 0.948 0.052
#&gt; GSM559390     1  0.2948      0.850 0.948 0.052
#&gt; GSM559392     2  0.6973      0.904 0.188 0.812
#&gt; GSM559393     1  0.0376      0.881 0.996 0.004
#&gt; GSM559394     1  0.0000      0.882 1.000 0.000
#&gt; GSM559396     1  0.0000      0.882 1.000 0.000
#&gt; GSM559398     2  0.6973      0.904 0.188 0.812
#&gt; GSM559399     1  0.0000      0.882 1.000 0.000
#&gt; GSM559400     2  0.9522      0.734 0.372 0.628
#&gt; GSM559402     1  0.0000      0.882 1.000 0.000
#&gt; GSM559403     1  0.0000      0.882 1.000 0.000
#&gt; GSM559404     1  0.0000      0.882 1.000 0.000
#&gt; GSM559405     1  0.2948      0.849 0.948 0.052
#&gt; GSM559406     1  0.0376      0.881 0.996 0.004
#&gt; GSM559407     1  0.0000      0.882 1.000 0.000
#&gt; GSM559408     1  0.0000      0.882 1.000 0.000
#&gt; GSM559409     1  0.0000      0.882 1.000 0.000
#&gt; GSM559410     1  0.0000      0.882 1.000 0.000
#&gt; GSM559411     1  0.0000      0.882 1.000 0.000
#&gt; GSM559412     1  0.0000      0.882 1.000 0.000
#&gt; GSM559413     1  0.0000      0.882 1.000 0.000
#&gt; GSM559415     1  0.2778      0.852 0.952 0.048
#&gt; GSM559416     1  0.3733      0.828 0.928 0.072
#&gt; GSM559417     1  0.3733      0.828 0.928 0.072
#&gt; GSM559418     1  0.2778      0.852 0.952 0.048
#&gt; GSM559419     1  0.0000      0.882 1.000 0.000
#&gt; GSM559420     1  0.0000      0.882 1.000 0.000
#&gt; GSM559421     2  0.6973      0.904 0.188 0.812
#&gt; GSM559423     2  0.7219      0.901 0.200 0.800
#&gt; GSM559425     2  0.6973      0.904 0.188 0.812
#&gt; GSM559426     2  0.7674      0.888 0.224 0.776
#&gt; GSM559427     2  0.6973      0.904 0.188 0.812
#&gt; GSM559428     2  0.9970      0.513 0.468 0.532
#&gt; GSM559429     2  0.9358      0.756 0.352 0.648
#&gt; GSM559430     2  0.6973      0.904 0.188 0.812
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559387     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559391     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559395     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559397     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559401     3  0.4750     0.7886 0.216 0.000 0.784
#&gt; GSM559414     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559422     3  0.0747     0.5675 0.000 0.016 0.984
#&gt; GSM559424     3  0.5810     0.8648 0.336 0.000 0.664
#&gt; GSM559431     2  0.0237     0.8463 0.000 0.996 0.004
#&gt; GSM559432     3  0.0747     0.5675 0.000 0.016 0.984
#&gt; GSM559381     1  0.2537     0.8707 0.920 0.080 0.000
#&gt; GSM559382     1  0.6520    -0.0811 0.508 0.488 0.004
#&gt; GSM559384     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559386     1  0.6148     0.3953 0.640 0.356 0.004
#&gt; GSM559388     2  0.5656     0.6357 0.284 0.712 0.004
#&gt; GSM559389     1  0.2537     0.8707 0.920 0.080 0.000
#&gt; GSM559390     1  0.1989     0.8981 0.948 0.048 0.004
#&gt; GSM559392     2  0.1289     0.8635 0.032 0.968 0.000
#&gt; GSM559393     1  0.1163     0.9129 0.972 0.028 0.000
#&gt; GSM559394     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559396     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559398     2  0.1289     0.8635 0.032 0.968 0.000
#&gt; GSM559399     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559400     2  0.5815     0.6102 0.304 0.692 0.004
#&gt; GSM559402     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559405     1  0.2537     0.8707 0.920 0.080 0.000
#&gt; GSM559406     1  0.0237     0.9263 0.996 0.000 0.004
#&gt; GSM559407     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559415     1  0.1878     0.8980 0.952 0.044 0.004
#&gt; GSM559416     1  0.2496     0.8776 0.928 0.068 0.004
#&gt; GSM559417     1  0.2496     0.8776 0.928 0.068 0.004
#&gt; GSM559418     1  0.1878     0.8980 0.952 0.044 0.004
#&gt; GSM559419     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000     0.9285 1.000 0.000 0.000
#&gt; GSM559421     2  0.1289     0.8635 0.032 0.968 0.000
#&gt; GSM559423     2  0.1765     0.8603 0.040 0.956 0.004
#&gt; GSM559425     2  0.0424     0.8551 0.008 0.992 0.000
#&gt; GSM559426     2  0.1753     0.8509 0.048 0.952 0.000
#&gt; GSM559427     2  0.0424     0.8551 0.008 0.992 0.000
#&gt; GSM559428     2  0.5785     0.5123 0.332 0.668 0.000
#&gt; GSM559429     2  0.4235     0.7204 0.176 0.824 0.000
#&gt; GSM559430     2  0.0424     0.8551 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559387     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559391     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559395     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559397     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559401     3  0.3978     0.7200 0.108 0.000 0.836 0.056
#&gt; GSM559414     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559422     4  0.4941     1.0000 0.000 0.000 0.436 0.564
#&gt; GSM559424     3  0.3486     0.9672 0.188 0.000 0.812 0.000
#&gt; GSM559431     2  0.2329     0.7593 0.000 0.916 0.012 0.072
#&gt; GSM559432     4  0.4941     1.0000 0.000 0.000 0.436 0.564
#&gt; GSM559381     1  0.5021     0.7200 0.724 0.036 0.000 0.240
#&gt; GSM559382     2  0.7968     0.3611 0.264 0.408 0.004 0.324
#&gt; GSM559384     1  0.0895     0.8408 0.976 0.000 0.004 0.020
#&gt; GSM559385     1  0.4646     0.7853 0.796 0.000 0.120 0.084
#&gt; GSM559386     1  0.7977     0.0417 0.412 0.280 0.004 0.304
#&gt; GSM559388     2  0.5988     0.6302 0.100 0.676 0.000 0.224
#&gt; GSM559389     1  0.5021     0.7200 0.724 0.036 0.000 0.240
#&gt; GSM559390     1  0.4126     0.7660 0.776 0.004 0.004 0.216
#&gt; GSM559392     2  0.1022     0.7942 0.000 0.968 0.000 0.032
#&gt; GSM559393     1  0.5680     0.7618 0.752 0.020 0.120 0.108
#&gt; GSM559394     1  0.4646     0.7853 0.796 0.000 0.120 0.084
#&gt; GSM559396     1  0.0895     0.8408 0.976 0.000 0.004 0.020
#&gt; GSM559398     2  0.1022     0.7942 0.000 0.968 0.000 0.032
#&gt; GSM559399     1  0.1022     0.8415 0.968 0.000 0.000 0.032
#&gt; GSM559400     2  0.6175     0.6233 0.092 0.664 0.004 0.240
#&gt; GSM559402     1  0.2149     0.8118 0.912 0.000 0.088 0.000
#&gt; GSM559403     1  0.1724     0.8426 0.948 0.000 0.020 0.032
#&gt; GSM559404     1  0.3658     0.7505 0.836 0.000 0.144 0.020
#&gt; GSM559405     1  0.5021     0.7200 0.724 0.036 0.000 0.240
#&gt; GSM559406     1  0.2773     0.8240 0.880 0.000 0.004 0.116
#&gt; GSM559407     1  0.2149     0.8118 0.912 0.000 0.088 0.000
#&gt; GSM559408     1  0.0336     0.8405 0.992 0.000 0.008 0.000
#&gt; GSM559409     1  0.0336     0.8405 0.992 0.000 0.008 0.000
#&gt; GSM559410     1  0.3082     0.8198 0.884 0.000 0.084 0.032
#&gt; GSM559411     1  0.0336     0.8405 0.992 0.000 0.008 0.000
#&gt; GSM559412     1  0.2216     0.8093 0.908 0.000 0.092 0.000
#&gt; GSM559413     1  0.2216     0.8093 0.908 0.000 0.092 0.000
#&gt; GSM559415     1  0.3501     0.8082 0.848 0.020 0.000 0.132
#&gt; GSM559416     1  0.4854     0.7365 0.732 0.020 0.004 0.244
#&gt; GSM559417     1  0.4854     0.7365 0.732 0.020 0.004 0.244
#&gt; GSM559418     1  0.3501     0.8082 0.848 0.020 0.000 0.132
#&gt; GSM559419     1  0.0376     0.8416 0.992 0.000 0.004 0.004
#&gt; GSM559420     1  0.0376     0.8416 0.992 0.000 0.004 0.004
#&gt; GSM559421     2  0.1022     0.7942 0.000 0.968 0.000 0.032
#&gt; GSM559423     2  0.1389     0.7932 0.000 0.952 0.000 0.048
#&gt; GSM559425     2  0.1767     0.7732 0.000 0.944 0.012 0.044
#&gt; GSM559426     2  0.3047     0.7629 0.012 0.872 0.000 0.116
#&gt; GSM559427     2  0.1767     0.7732 0.000 0.944 0.012 0.044
#&gt; GSM559428     2  0.7117     0.5360 0.196 0.584 0.004 0.216
#&gt; GSM559429     2  0.5574     0.6709 0.092 0.732 0.004 0.172
#&gt; GSM559430     2  0.0188     0.7895 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559387     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559391     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559395     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559397     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559401     3  0.2077      0.849 0.000 0.000 0.920 0.040 0.040
#&gt; GSM559414     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559422     5  0.4666      1.000 0.000 0.000 0.040 0.284 0.676
#&gt; GSM559424     3  0.1043      0.981 0.040 0.000 0.960 0.000 0.000
#&gt; GSM559431     2  0.2293      0.734 0.000 0.900 0.000 0.084 0.016
#&gt; GSM559432     5  0.4666      1.000 0.000 0.000 0.040 0.284 0.676
#&gt; GSM559381     1  0.4761      0.299 0.616 0.028 0.000 0.356 0.000
#&gt; GSM559382     4  0.6272      0.579 0.152 0.380 0.000 0.468 0.000
#&gt; GSM559384     1  0.1430      0.713 0.944 0.000 0.000 0.052 0.004
#&gt; GSM559385     1  0.6448      0.494 0.560 0.000 0.052 0.076 0.312
#&gt; GSM559386     4  0.6641      0.681 0.296 0.256 0.000 0.448 0.000
#&gt; GSM559388     2  0.4522      0.280 0.024 0.660 0.000 0.316 0.000
#&gt; GSM559389     1  0.4761      0.299 0.616 0.028 0.000 0.356 0.000
#&gt; GSM559390     1  0.3857      0.472 0.688 0.000 0.000 0.312 0.000
#&gt; GSM559392     2  0.1197      0.763 0.000 0.952 0.000 0.048 0.000
#&gt; GSM559393     1  0.7285      0.422 0.508 0.016 0.052 0.112 0.312
#&gt; GSM559394     1  0.6448      0.494 0.560 0.000 0.052 0.076 0.312
#&gt; GSM559396     1  0.1410      0.710 0.940 0.000 0.000 0.060 0.000
#&gt; GSM559398     2  0.1197      0.763 0.000 0.952 0.000 0.048 0.000
#&gt; GSM559399     1  0.1469      0.721 0.948 0.000 0.000 0.036 0.016
#&gt; GSM559400     2  0.4467      0.277 0.016 0.640 0.000 0.344 0.000
#&gt; GSM559402     1  0.2389      0.701 0.880 0.000 0.004 0.000 0.116
#&gt; GSM559403     1  0.2157      0.724 0.920 0.000 0.004 0.036 0.040
#&gt; GSM559404     1  0.5654      0.474 0.592 0.000 0.076 0.008 0.324
#&gt; GSM559405     1  0.4761      0.299 0.616 0.028 0.000 0.356 0.000
#&gt; GSM559406     1  0.3048      0.639 0.820 0.000 0.000 0.176 0.004
#&gt; GSM559407     1  0.2389      0.701 0.880 0.000 0.004 0.000 0.116
#&gt; GSM559408     1  0.0451      0.724 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559409     1  0.0451      0.724 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559410     1  0.3222      0.707 0.852 0.000 0.004 0.036 0.108
#&gt; GSM559411     1  0.0609      0.724 0.980 0.000 0.000 0.000 0.020
#&gt; GSM559412     1  0.2970      0.677 0.828 0.000 0.004 0.000 0.168
#&gt; GSM559413     1  0.2970      0.677 0.828 0.000 0.004 0.000 0.168
#&gt; GSM559415     1  0.3779      0.583 0.752 0.012 0.000 0.236 0.000
#&gt; GSM559416     1  0.4565      0.276 0.580 0.012 0.000 0.408 0.000
#&gt; GSM559417     1  0.4565      0.276 0.580 0.012 0.000 0.408 0.000
#&gt; GSM559418     1  0.3779      0.583 0.752 0.012 0.000 0.236 0.000
#&gt; GSM559419     1  0.0290      0.724 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559420     1  0.0290      0.724 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559421     2  0.1341      0.763 0.000 0.944 0.000 0.056 0.000
#&gt; GSM559423     2  0.1544      0.760 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559425     2  0.1478      0.748 0.000 0.936 0.000 0.064 0.000
#&gt; GSM559426     2  0.2864      0.684 0.012 0.852 0.000 0.136 0.000
#&gt; GSM559427     2  0.1478      0.748 0.000 0.936 0.000 0.064 0.000
#&gt; GSM559428     2  0.6054     -0.226 0.148 0.548 0.000 0.304 0.000
#&gt; GSM559429     2  0.4873      0.403 0.068 0.688 0.000 0.244 0.000
#&gt; GSM559430     2  0.0794      0.764 0.000 0.972 0.000 0.028 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4   p5    p6
#&gt; GSM559383     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559387     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559391     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559395     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559397     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559401     3  0.2048     0.8632 0.000 0.000 0.880 0.000 0.12 0.000
#&gt; GSM559414     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559422     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM559424     3  0.0000     0.9824 0.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM559431     2  0.4626     0.5726 0.000 0.692 0.000 0.172 0.00 0.136
#&gt; GSM559432     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM559381     1  0.5975     0.2058 0.580 0.028 0.012 0.116 0.00 0.264
#&gt; GSM559382     2  0.7510    -0.1912 0.116 0.396 0.012 0.196 0.00 0.280
#&gt; GSM559384     1  0.1682     0.5251 0.928 0.000 0.000 0.052 0.00 0.020
#&gt; GSM559385     1  0.3868    -0.8547 0.504 0.000 0.000 0.496 0.00 0.000
#&gt; GSM559386     6  0.7941     0.0887 0.260 0.264 0.012 0.184 0.00 0.280
#&gt; GSM559388     2  0.4838     0.4160 0.004 0.676 0.000 0.192 0.00 0.128
#&gt; GSM559389     1  0.5975     0.2058 0.580 0.028 0.012 0.116 0.00 0.264
#&gt; GSM559390     1  0.4785     0.3210 0.660 0.004 0.012 0.052 0.00 0.272
#&gt; GSM559392     2  0.0000     0.6803 0.000 1.000 0.000 0.000 0.00 0.000
#&gt; GSM559393     4  0.4869     0.0000 0.456 0.024 0.000 0.500 0.00 0.020
#&gt; GSM559394     1  0.3868    -0.8547 0.504 0.000 0.000 0.496 0.00 0.000
#&gt; GSM559396     1  0.1845     0.5251 0.920 0.000 0.000 0.052 0.00 0.028
#&gt; GSM559398     2  0.0260     0.6804 0.000 0.992 0.000 0.000 0.00 0.008
#&gt; GSM559399     1  0.1501     0.4968 0.924 0.000 0.000 0.076 0.00 0.000
#&gt; GSM559400     2  0.4949     0.4253 0.004 0.644 0.000 0.104 0.00 0.248
#&gt; GSM559402     1  0.2135     0.3783 0.872 0.000 0.000 0.128 0.00 0.000
#&gt; GSM559403     1  0.1814     0.4735 0.900 0.000 0.000 0.100 0.00 0.000
#&gt; GSM559404     1  0.3862    -0.6616 0.524 0.000 0.000 0.476 0.00 0.000
#&gt; GSM559405     1  0.5975     0.2058 0.580 0.028 0.012 0.116 0.00 0.264
#&gt; GSM559406     1  0.3477     0.4549 0.808 0.004 0.000 0.056 0.00 0.132
#&gt; GSM559407     1  0.2135     0.3783 0.872 0.000 0.000 0.128 0.00 0.000
#&gt; GSM559408     1  0.0520     0.5303 0.984 0.000 0.000 0.008 0.00 0.008
#&gt; GSM559409     1  0.0520     0.5303 0.984 0.000 0.000 0.008 0.00 0.008
#&gt; GSM559410     1  0.2562     0.3537 0.828 0.000 0.000 0.172 0.00 0.000
#&gt; GSM559411     1  0.1010     0.5186 0.960 0.000 0.000 0.036 0.00 0.004
#&gt; GSM559412     1  0.2838     0.2601 0.808 0.000 0.000 0.188 0.00 0.004
#&gt; GSM559413     1  0.2871     0.2505 0.804 0.000 0.000 0.192 0.00 0.004
#&gt; GSM559415     1  0.4295     0.3905 0.720 0.020 0.000 0.224 0.00 0.036
#&gt; GSM559416     1  0.5930     0.2681 0.564 0.024 0.000 0.192 0.00 0.220
#&gt; GSM559417     1  0.5930     0.2681 0.564 0.024 0.000 0.192 0.00 0.220
#&gt; GSM559418     1  0.4295     0.3905 0.720 0.020 0.000 0.224 0.00 0.036
#&gt; GSM559419     1  0.0405     0.5299 0.988 0.000 0.000 0.008 0.00 0.004
#&gt; GSM559420     1  0.0405     0.5299 0.988 0.000 0.000 0.008 0.00 0.004
#&gt; GSM559421     2  0.0937     0.6677 0.000 0.960 0.000 0.000 0.00 0.040
#&gt; GSM559423     2  0.2747     0.6127 0.000 0.860 0.000 0.044 0.00 0.096
#&gt; GSM559425     2  0.3901     0.6236 0.000 0.768 0.000 0.136 0.00 0.096
#&gt; GSM559426     6  0.4833     0.1237 0.000 0.428 0.000 0.056 0.00 0.516
#&gt; GSM559427     2  0.3901     0.6236 0.000 0.768 0.000 0.136 0.00 0.096
#&gt; GSM559428     6  0.5183     0.4180 0.120 0.176 0.012 0.012 0.00 0.680
#&gt; GSM559429     6  0.4332     0.3735 0.000 0.228 0.000 0.072 0.00 0.700
#&gt; GSM559430     2  0.2106     0.6670 0.000 0.904 0.000 0.032 0.00 0.064
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:hclust 50         2.90e-01 2
#> CV:hclust 50         2.05e-10 3
#> CV:hclust 50         1.11e-09 4
#> CV:hclust 38         8.67e-07 5
#> CV:hclust 25         8.49e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.497           0.756       0.795         0.3757 0.581   0.581
#> 3 3 1.000           0.975       0.981         0.6180 0.793   0.647
#> 4 4 0.677           0.706       0.824         0.1463 0.931   0.825
#> 5 5 0.659           0.592       0.743         0.0882 0.923   0.777
#> 6 6 0.674           0.544       0.702         0.0667 0.836   0.485
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.118      0.445 0.984 0.016
#&gt; GSM559387     1   0.118      0.445 0.984 0.016
#&gt; GSM559391     1   0.118      0.445 0.984 0.016
#&gt; GSM559395     1   0.118      0.445 0.984 0.016
#&gt; GSM559397     1   0.118      0.445 0.984 0.016
#&gt; GSM559401     1   0.118      0.445 0.984 0.016
#&gt; GSM559414     1   0.118      0.445 0.984 0.016
#&gt; GSM559422     1   0.343      0.387 0.936 0.064
#&gt; GSM559424     1   0.118      0.445 0.984 0.016
#&gt; GSM559431     2   0.000      0.936 0.000 1.000
#&gt; GSM559432     2   0.993      0.347 0.452 0.548
#&gt; GSM559381     1   0.993      0.787 0.548 0.452
#&gt; GSM559382     2   0.141      0.911 0.020 0.980
#&gt; GSM559384     1   0.993      0.787 0.548 0.452
#&gt; GSM559385     1   0.993      0.787 0.548 0.452
#&gt; GSM559386     1   0.995      0.776 0.540 0.460
#&gt; GSM559388     2   0.000      0.936 0.000 1.000
#&gt; GSM559389     1   0.993      0.787 0.548 0.452
#&gt; GSM559390     1   0.993      0.787 0.548 0.452
#&gt; GSM559392     2   0.000      0.936 0.000 1.000
#&gt; GSM559393     1   0.994      0.782 0.544 0.456
#&gt; GSM559394     1   0.993      0.787 0.548 0.452
#&gt; GSM559396     1   0.993      0.787 0.548 0.452
#&gt; GSM559398     2   0.000      0.936 0.000 1.000
#&gt; GSM559399     1   0.993      0.787 0.548 0.452
#&gt; GSM559400     2   0.141      0.909 0.020 0.980
#&gt; GSM559402     1   0.993      0.787 0.548 0.452
#&gt; GSM559403     1   0.993      0.787 0.548 0.452
#&gt; GSM559404     1   0.990      0.781 0.560 0.440
#&gt; GSM559405     1   0.993      0.787 0.548 0.452
#&gt; GSM559406     1   0.990      0.781 0.560 0.440
#&gt; GSM559407     1   0.993      0.787 0.548 0.452
#&gt; GSM559408     1   0.993      0.787 0.548 0.452
#&gt; GSM559409     1   0.993      0.787 0.548 0.452
#&gt; GSM559410     1   0.993      0.787 0.548 0.452
#&gt; GSM559411     1   0.990      0.781 0.560 0.440
#&gt; GSM559412     1   0.990      0.781 0.560 0.440
#&gt; GSM559413     1   0.990      0.781 0.560 0.440
#&gt; GSM559415     1   0.993      0.787 0.548 0.452
#&gt; GSM559416     1   0.993      0.787 0.548 0.452
#&gt; GSM559417     1   0.993      0.787 0.548 0.452
#&gt; GSM559418     1   0.994      0.782 0.544 0.456
#&gt; GSM559419     1   0.993      0.787 0.548 0.452
#&gt; GSM559420     1   0.993      0.787 0.548 0.452
#&gt; GSM559421     2   0.000      0.936 0.000 1.000
#&gt; GSM559423     2   0.000      0.936 0.000 1.000
#&gt; GSM559425     2   0.000      0.936 0.000 1.000
#&gt; GSM559426     2   0.000      0.936 0.000 1.000
#&gt; GSM559427     2   0.000      0.936 0.000 1.000
#&gt; GSM559428     2   0.141      0.911 0.020 0.980
#&gt; GSM559429     2   0.000      0.936 0.000 1.000
#&gt; GSM559430     2   0.000      0.936 0.000 1.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559387     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559391     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559395     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559397     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559401     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559414     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559422     3  0.0424      0.954 0.008 0.000 0.992
#&gt; GSM559424     3  0.1643      0.988 0.044 0.000 0.956
#&gt; GSM559431     2  0.0424      0.964 0.000 0.992 0.008
#&gt; GSM559432     3  0.0237      0.944 0.000 0.004 0.996
#&gt; GSM559381     1  0.0237      0.990 0.996 0.004 0.000
#&gt; GSM559382     2  0.3039      0.923 0.044 0.920 0.036
#&gt; GSM559384     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559386     1  0.1832      0.958 0.956 0.008 0.036
#&gt; GSM559388     2  0.1411      0.953 0.000 0.964 0.036
#&gt; GSM559389     1  0.0237      0.990 0.996 0.004 0.000
#&gt; GSM559390     1  0.0592      0.984 0.988 0.000 0.012
#&gt; GSM559392     2  0.0000      0.964 0.000 1.000 0.000
#&gt; GSM559393     1  0.1832      0.958 0.956 0.008 0.036
#&gt; GSM559394     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559396     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559398     2  0.0424      0.964 0.000 0.992 0.008
#&gt; GSM559399     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559400     2  0.4915      0.806 0.132 0.832 0.036
#&gt; GSM559402     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559417     1  0.1647      0.961 0.960 0.004 0.036
#&gt; GSM559418     1  0.1832      0.958 0.956 0.008 0.036
#&gt; GSM559419     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.964 0.000 1.000 0.000
#&gt; GSM559423     2  0.1031      0.959 0.000 0.976 0.024
#&gt; GSM559425     2  0.0424      0.964 0.000 0.992 0.008
#&gt; GSM559426     2  0.0000      0.964 0.000 1.000 0.000
#&gt; GSM559427     2  0.0424      0.964 0.000 0.992 0.008
#&gt; GSM559428     2  0.3039      0.923 0.044 0.920 0.036
#&gt; GSM559429     2  0.1031      0.959 0.000 0.976 0.024
#&gt; GSM559430     2  0.0424      0.964 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0188    0.95679 0.000 0.000 0.996 0.004
#&gt; GSM559387     3  0.0000    0.95751 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0188    0.95679 0.000 0.000 0.996 0.004
#&gt; GSM559395     3  0.0000    0.95751 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000    0.95751 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0000    0.95751 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000    0.95751 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.4008    0.81993 0.000 0.000 0.756 0.244
#&gt; GSM559424     3  0.0188    0.95679 0.000 0.000 0.996 0.004
#&gt; GSM559431     2  0.0336    0.84863 0.000 0.992 0.000 0.008
#&gt; GSM559432     3  0.4008    0.81993 0.000 0.000 0.756 0.244
#&gt; GSM559381     1  0.4454    0.74220 0.692 0.000 0.000 0.308
#&gt; GSM559382     4  0.5508   -0.03453 0.016 0.476 0.000 0.508
#&gt; GSM559384     1  0.3356    0.79573 0.824 0.000 0.000 0.176
#&gt; GSM559385     1  0.4356    0.75152 0.708 0.000 0.000 0.292
#&gt; GSM559386     4  0.4543   -0.00163 0.324 0.000 0.000 0.676
#&gt; GSM559388     2  0.4925    0.16550 0.000 0.572 0.000 0.428
#&gt; GSM559389     1  0.4624    0.71270 0.660 0.000 0.000 0.340
#&gt; GSM559390     1  0.4250    0.42975 0.724 0.000 0.000 0.276
#&gt; GSM559392     2  0.3123    0.82103 0.000 0.844 0.000 0.156
#&gt; GSM559393     4  0.4564   -0.04922 0.328 0.000 0.000 0.672
#&gt; GSM559394     1  0.4382    0.75035 0.704 0.000 0.000 0.296
#&gt; GSM559396     1  0.3610    0.79439 0.800 0.000 0.000 0.200
#&gt; GSM559398     2  0.0817    0.85204 0.000 0.976 0.000 0.024
#&gt; GSM559399     1  0.4522    0.74240 0.680 0.000 0.000 0.320
#&gt; GSM559400     4  0.7412    0.12340 0.168 0.388 0.000 0.444
#&gt; GSM559402     1  0.3219    0.79590 0.836 0.000 0.000 0.164
#&gt; GSM559403     1  0.4331    0.75364 0.712 0.000 0.000 0.288
#&gt; GSM559404     1  0.4040    0.78065 0.752 0.000 0.000 0.248
#&gt; GSM559405     1  0.3907    0.77854 0.768 0.000 0.000 0.232
#&gt; GSM559406     1  0.0592    0.75015 0.984 0.000 0.000 0.016
#&gt; GSM559407     1  0.3074    0.79496 0.848 0.000 0.000 0.152
#&gt; GSM559408     1  0.0000    0.75891 1.000 0.000 0.000 0.000
#&gt; GSM559409     1  0.0000    0.75891 1.000 0.000 0.000 0.000
#&gt; GSM559410     1  0.3764    0.78504 0.784 0.000 0.000 0.216
#&gt; GSM559411     1  0.0707    0.75466 0.980 0.000 0.000 0.020
#&gt; GSM559412     1  0.0707    0.75466 0.980 0.000 0.000 0.020
#&gt; GSM559413     1  0.0707    0.75466 0.980 0.000 0.000 0.020
#&gt; GSM559415     1  0.4522    0.74170 0.680 0.000 0.000 0.320
#&gt; GSM559416     1  0.3444    0.59305 0.816 0.000 0.000 0.184
#&gt; GSM559417     1  0.3688    0.55254 0.792 0.000 0.000 0.208
#&gt; GSM559418     1  0.4989    0.51813 0.528 0.000 0.000 0.472
#&gt; GSM559419     1  0.2281    0.72587 0.904 0.000 0.000 0.096
#&gt; GSM559420     1  0.3219    0.79601 0.836 0.000 0.000 0.164
#&gt; GSM559421     2  0.2345    0.85013 0.000 0.900 0.000 0.100
#&gt; GSM559423     2  0.3266    0.80943 0.000 0.832 0.000 0.168
#&gt; GSM559425     2  0.0000    0.85143 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.2469    0.84723 0.000 0.892 0.000 0.108
#&gt; GSM559427     2  0.0000    0.85143 0.000 1.000 0.000 0.000
#&gt; GSM559428     4  0.5510   -0.06143 0.016 0.480 0.000 0.504
#&gt; GSM559429     2  0.3266    0.81293 0.000 0.832 0.000 0.168
#&gt; GSM559430     2  0.0000    0.85143 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM559383     3  0.0609    0.90963 0.000 0.000 0.980 0.020 NA
#&gt; GSM559387     3  0.0000    0.91239 0.000 0.000 1.000 0.000 NA
#&gt; GSM559391     3  0.0609    0.90963 0.000 0.000 0.980 0.020 NA
#&gt; GSM559395     3  0.0000    0.91239 0.000 0.000 1.000 0.000 NA
#&gt; GSM559397     3  0.0000    0.91239 0.000 0.000 1.000 0.000 NA
#&gt; GSM559401     3  0.0000    0.91239 0.000 0.000 1.000 0.000 NA
#&gt; GSM559414     3  0.0000    0.91239 0.000 0.000 1.000 0.000 NA
#&gt; GSM559422     3  0.4425    0.61454 0.000 0.000 0.544 0.004 NA
#&gt; GSM559424     3  0.0609    0.90963 0.000 0.000 0.980 0.020 NA
#&gt; GSM559431     2  0.0671    0.81506 0.000 0.980 0.000 0.004 NA
#&gt; GSM559432     3  0.4641    0.60324 0.000 0.000 0.532 0.012 NA
#&gt; GSM559381     1  0.3779    0.58047 0.804 0.000 0.000 0.144 NA
#&gt; GSM559382     4  0.6165    0.27890 0.040 0.220 0.000 0.632 NA
#&gt; GSM559384     1  0.1568    0.66097 0.944 0.000 0.000 0.036 NA
#&gt; GSM559385     1  0.4890    0.54902 0.680 0.000 0.000 0.064 NA
#&gt; GSM559386     4  0.5839    0.43850 0.248 0.004 0.000 0.612 NA
#&gt; GSM559388     4  0.5825    0.06219 0.000 0.320 0.000 0.564 NA
#&gt; GSM559389     1  0.4934    0.55929 0.708 0.000 0.000 0.104 NA
#&gt; GSM559390     4  0.3957    0.19026 0.280 0.000 0.000 0.712 NA
#&gt; GSM559392     2  0.5305    0.72280 0.000 0.672 0.000 0.196 NA
#&gt; GSM559393     4  0.6796    0.02692 0.328 0.000 0.000 0.376 NA
#&gt; GSM559394     1  0.5059    0.54006 0.668 0.000 0.000 0.076 NA
#&gt; GSM559396     1  0.3002    0.63416 0.856 0.000 0.000 0.116 NA
#&gt; GSM559398     2  0.1661    0.81446 0.000 0.940 0.000 0.036 NA
#&gt; GSM559399     1  0.4836    0.56873 0.716 0.000 0.000 0.096 NA
#&gt; GSM559400     4  0.4733    0.35025 0.008 0.152 0.000 0.748 NA
#&gt; GSM559402     1  0.1670    0.65783 0.936 0.000 0.000 0.052 NA
#&gt; GSM559403     1  0.4890    0.54902 0.680 0.000 0.000 0.064 NA
#&gt; GSM559404     1  0.4298    0.60158 0.756 0.000 0.000 0.060 NA
#&gt; GSM559405     1  0.2573    0.64534 0.880 0.000 0.000 0.016 NA
#&gt; GSM559406     1  0.5027    0.49735 0.640 0.000 0.000 0.304 NA
#&gt; GSM559407     1  0.1764    0.65655 0.928 0.000 0.000 0.064 NA
#&gt; GSM559408     1  0.4967    0.51980 0.660 0.000 0.000 0.280 NA
#&gt; GSM559409     1  0.4878    0.52737 0.676 0.000 0.000 0.264 NA
#&gt; GSM559410     1  0.3565    0.63029 0.800 0.000 0.000 0.024 NA
#&gt; GSM559411     1  0.4890    0.52760 0.680 0.000 0.000 0.256 NA
#&gt; GSM559412     1  0.4937    0.52620 0.672 0.000 0.000 0.264 NA
#&gt; GSM559413     1  0.4914    0.52756 0.676 0.000 0.000 0.260 NA
#&gt; GSM559415     1  0.4998    0.55713 0.700 0.000 0.000 0.104 NA
#&gt; GSM559416     4  0.4930   -0.03626 0.388 0.000 0.000 0.580 NA
#&gt; GSM559417     4  0.4898    0.00102 0.376 0.000 0.000 0.592 NA
#&gt; GSM559418     1  0.6102    0.33691 0.568 0.000 0.000 0.232 NA
#&gt; GSM559419     1  0.4966    0.36040 0.564 0.000 0.000 0.404 NA
#&gt; GSM559420     1  0.2325    0.65256 0.904 0.000 0.000 0.068 NA
#&gt; GSM559421     2  0.4351    0.79447 0.000 0.768 0.000 0.132 NA
#&gt; GSM559423     2  0.5367    0.74616 0.000 0.668 0.000 0.184 NA
#&gt; GSM559425     2  0.0404    0.81718 0.000 0.988 0.000 0.000 NA
#&gt; GSM559426     2  0.4764    0.78151 0.000 0.732 0.000 0.140 NA
#&gt; GSM559427     2  0.0404    0.81718 0.000 0.988 0.000 0.000 NA
#&gt; GSM559428     4  0.6867    0.21261 0.048 0.224 0.000 0.560 NA
#&gt; GSM559429     2  0.5109    0.75723 0.000 0.696 0.000 0.172 NA
#&gt; GSM559430     2  0.0510    0.81793 0.000 0.984 0.000 0.000 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0622     0.9766 0.012 0.000 0.980 0.000 0.000 0.008
#&gt; GSM559387     3  0.0000     0.9829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0622     0.9766 0.012 0.000 0.980 0.000 0.000 0.008
#&gt; GSM559395     3  0.0146     0.9829 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000     0.9829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0260     0.9759 0.008 0.000 0.992 0.000 0.000 0.000
#&gt; GSM559414     3  0.0000     0.9829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.3862     0.9826 0.000 0.000 0.476 0.000 0.524 0.000
#&gt; GSM559424     3  0.0622     0.9766 0.012 0.000 0.980 0.000 0.000 0.008
#&gt; GSM559431     2  0.1003     0.6895 0.000 0.964 0.000 0.000 0.016 0.020
#&gt; GSM559432     5  0.3857     0.9828 0.000 0.000 0.468 0.000 0.532 0.000
#&gt; GSM559381     1  0.6917     0.1100 0.376 0.000 0.000 0.364 0.072 0.188
#&gt; GSM559382     6  0.3150     0.5791 0.104 0.064 0.000 0.000 0.000 0.832
#&gt; GSM559384     4  0.5310     0.1550 0.332 0.000 0.000 0.576 0.072 0.020
#&gt; GSM559385     1  0.4606     0.6579 0.708 0.000 0.000 0.208 0.064 0.020
#&gt; GSM559386     6  0.3192     0.5573 0.216 0.000 0.000 0.004 0.004 0.776
#&gt; GSM559388     6  0.3852     0.5272 0.064 0.120 0.000 0.000 0.020 0.796
#&gt; GSM559389     1  0.5606     0.5907 0.644 0.000 0.000 0.192 0.060 0.104
#&gt; GSM559390     6  0.7139     0.0615 0.116 0.000 0.000 0.328 0.164 0.392
#&gt; GSM559392     2  0.4636     0.3915 0.004 0.532 0.000 0.000 0.032 0.432
#&gt; GSM559393     1  0.4317     0.5117 0.740 0.000 0.000 0.016 0.064 0.180
#&gt; GSM559394     1  0.4170     0.6653 0.740 0.000 0.000 0.192 0.060 0.008
#&gt; GSM559396     4  0.6266     0.2096 0.292 0.000 0.000 0.532 0.084 0.092
#&gt; GSM559398     2  0.2750     0.6509 0.000 0.844 0.000 0.000 0.020 0.136
#&gt; GSM559399     1  0.3753     0.6446 0.788 0.000 0.000 0.156 0.040 0.016
#&gt; GSM559400     6  0.5523     0.5190 0.060 0.032 0.000 0.052 0.168 0.688
#&gt; GSM559402     4  0.4978     0.2780 0.268 0.000 0.000 0.644 0.072 0.016
#&gt; GSM559403     1  0.4213     0.6519 0.708 0.000 0.000 0.240 0.048 0.004
#&gt; GSM559404     4  0.5282     0.1470 0.244 0.000 0.000 0.636 0.096 0.024
#&gt; GSM559405     1  0.5285     0.2695 0.488 0.000 0.000 0.436 0.060 0.016
#&gt; GSM559406     4  0.2870     0.5362 0.004 0.000 0.000 0.856 0.100 0.040
#&gt; GSM559407     4  0.4729     0.3004 0.256 0.000 0.000 0.668 0.064 0.012
#&gt; GSM559408     4  0.1923     0.5595 0.004 0.000 0.000 0.916 0.064 0.016
#&gt; GSM559409     4  0.1196     0.5678 0.000 0.000 0.000 0.952 0.040 0.008
#&gt; GSM559410     1  0.3706     0.5353 0.620 0.000 0.000 0.380 0.000 0.000
#&gt; GSM559411     4  0.0508     0.5653 0.012 0.000 0.000 0.984 0.004 0.000
#&gt; GSM559412     4  0.1010     0.5670 0.004 0.000 0.000 0.960 0.036 0.000
#&gt; GSM559413     4  0.0260     0.5642 0.000 0.000 0.000 0.992 0.008 0.000
#&gt; GSM559415     1  0.3473     0.6443 0.812 0.000 0.000 0.136 0.040 0.012
#&gt; GSM559416     4  0.7386     0.1414 0.200 0.000 0.000 0.412 0.188 0.200
#&gt; GSM559417     4  0.7396     0.1085 0.188 0.000 0.000 0.408 0.188 0.216
#&gt; GSM559418     1  0.3236     0.6061 0.852 0.000 0.000 0.060 0.040 0.048
#&gt; GSM559419     4  0.7074     0.2656 0.268 0.000 0.000 0.444 0.168 0.120
#&gt; GSM559420     4  0.5356     0.1955 0.348 0.000 0.000 0.560 0.072 0.020
#&gt; GSM559421     2  0.4446     0.4902 0.004 0.600 0.000 0.000 0.028 0.368
#&gt; GSM559423     6  0.5503    -0.4437 0.016 0.448 0.000 0.000 0.080 0.456
#&gt; GSM559425     2  0.0146     0.7032 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559426     2  0.5504     0.4573 0.024 0.564 0.000 0.000 0.084 0.328
#&gt; GSM559427     2  0.0146     0.7032 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559428     6  0.4167     0.4969 0.072 0.056 0.000 0.000 0.084 0.788
#&gt; GSM559429     2  0.5992     0.3351 0.032 0.464 0.000 0.000 0.108 0.396
#&gt; GSM559430     2  0.0291     0.7036 0.000 0.992 0.000 0.000 0.004 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:kmeans 42         7.20e-01 2
#> CV:kmeans 52         8.27e-11 3
#> CV:kmeans 45         1.93e-09 4
#> CV:kmeans 40         1.97e-08 5
#> CV:kmeans 34         1.26e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.524           0.899       0.912         0.4755 0.517   0.517
#> 3 3 1.000           0.969       0.989         0.3554 0.773   0.586
#> 4 4 0.847           0.907       0.931         0.1694 0.851   0.600
#> 5 5 0.771           0.690       0.843         0.0574 0.913   0.674
#> 6 6 0.759           0.539       0.765         0.0349 0.956   0.797
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.6801      0.845 0.820 0.180
#&gt; GSM559387     1  0.6801      0.845 0.820 0.180
#&gt; GSM559391     1  0.6801      0.845 0.820 0.180
#&gt; GSM559395     1  0.6801      0.845 0.820 0.180
#&gt; GSM559397     1  0.6801      0.845 0.820 0.180
#&gt; GSM559401     1  0.6801      0.845 0.820 0.180
#&gt; GSM559414     1  0.6801      0.845 0.820 0.180
#&gt; GSM559422     2  0.1414      0.858 0.020 0.980
#&gt; GSM559424     1  0.6801      0.845 0.820 0.180
#&gt; GSM559431     2  0.0000      0.871 0.000 1.000
#&gt; GSM559432     2  0.1414      0.858 0.020 0.980
#&gt; GSM559381     1  0.1414      0.931 0.980 0.020
#&gt; GSM559382     2  0.5408      0.928 0.124 0.876
#&gt; GSM559384     1  0.1414      0.931 0.980 0.020
#&gt; GSM559385     1  0.1414      0.931 0.980 0.020
#&gt; GSM559386     2  0.6801      0.889 0.180 0.820
#&gt; GSM559388     2  0.5842      0.919 0.140 0.860
#&gt; GSM559389     1  0.1414      0.931 0.980 0.020
#&gt; GSM559390     1  0.0000      0.930 1.000 0.000
#&gt; GSM559392     2  0.5408      0.928 0.124 0.876
#&gt; GSM559393     2  0.6801      0.889 0.180 0.820
#&gt; GSM559394     1  0.1414      0.931 0.980 0.020
#&gt; GSM559396     1  0.5519      0.867 0.872 0.128
#&gt; GSM559398     2  0.5408      0.928 0.124 0.876
#&gt; GSM559399     1  0.1414      0.931 0.980 0.020
#&gt; GSM559400     2  0.3584      0.889 0.068 0.932
#&gt; GSM559402     1  0.1414      0.931 0.980 0.020
#&gt; GSM559403     1  0.1414      0.931 0.980 0.020
#&gt; GSM559404     1  0.3274      0.906 0.940 0.060
#&gt; GSM559405     1  0.1414      0.931 0.980 0.020
#&gt; GSM559406     1  0.0000      0.930 1.000 0.000
#&gt; GSM559407     1  0.1414      0.931 0.980 0.020
#&gt; GSM559408     1  0.0672      0.931 0.992 0.008
#&gt; GSM559409     1  0.0672      0.931 0.992 0.008
#&gt; GSM559410     1  0.1414      0.931 0.980 0.020
#&gt; GSM559411     1  0.0000      0.930 1.000 0.000
#&gt; GSM559412     1  0.0000      0.930 1.000 0.000
#&gt; GSM559413     1  0.2603      0.913 0.956 0.044
#&gt; GSM559415     1  0.1414      0.931 0.980 0.020
#&gt; GSM559416     1  0.0672      0.931 0.992 0.008
#&gt; GSM559417     2  0.9552      0.607 0.376 0.624
#&gt; GSM559418     2  0.6801      0.889 0.180 0.820
#&gt; GSM559419     1  0.1414      0.931 0.980 0.020
#&gt; GSM559420     1  0.1414      0.931 0.980 0.020
#&gt; GSM559421     2  0.5408      0.928 0.124 0.876
#&gt; GSM559423     2  0.5408      0.928 0.124 0.876
#&gt; GSM559425     2  0.5408      0.928 0.124 0.876
#&gt; GSM559426     2  0.5408      0.928 0.124 0.876
#&gt; GSM559427     2  0.5408      0.928 0.124 0.876
#&gt; GSM559428     2  0.0000      0.871 0.000 1.000
#&gt; GSM559429     2  0.0000      0.871 0.000 1.000
#&gt; GSM559430     2  0.5408      0.928 0.124 0.876
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3
#&gt; GSM559383     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559387     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559391     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559395     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559397     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559401     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559414     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559422     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559424     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559431     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559432     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559381     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559382     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559384     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559385     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559386     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559388     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559389     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559390     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559392     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559393     2   0.362      0.809 0.136 0.864  0
#&gt; GSM559394     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559396     3   0.000      1.000 0.000 0.000  1
#&gt; GSM559398     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559399     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559400     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559402     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559403     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559404     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559405     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559406     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559407     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559408     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559409     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559410     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559411     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559412     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559413     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559415     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559416     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559417     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559418     2   0.623      0.250 0.436 0.564  0
#&gt; GSM559419     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559420     1   0.000      1.000 1.000 0.000  0
#&gt; GSM559421     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559423     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559425     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559426     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559427     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559428     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559429     2   0.000      0.956 0.000 1.000  0
#&gt; GSM559430     2   0.000      0.956 0.000 1.000  0
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3    p4
#&gt; GSM559383     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559387     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559391     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559395     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559397     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559401     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559414     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559422     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559424     3  0.0000      0.989 0.000 0.000 1.00 0.000
#&gt; GSM559431     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559432     3  0.1637      0.932 0.000 0.060 0.94 0.000
#&gt; GSM559381     1  0.2921      0.830 0.860 0.000 0.00 0.140
#&gt; GSM559382     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559384     1  0.3311      0.806 0.828 0.000 0.00 0.172
#&gt; GSM559385     1  0.0000      0.848 1.000 0.000 0.00 0.000
#&gt; GSM559386     2  0.2775      0.894 0.084 0.896 0.00 0.020
#&gt; GSM559388     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559389     1  0.0707      0.851 0.980 0.000 0.00 0.020
#&gt; GSM559390     4  0.0707      0.917 0.020 0.000 0.00 0.980
#&gt; GSM559392     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559393     1  0.1389      0.826 0.952 0.048 0.00 0.000
#&gt; GSM559394     1  0.0336      0.847 0.992 0.000 0.00 0.008
#&gt; GSM559396     3  0.1211      0.954 0.000 0.000 0.96 0.040
#&gt; GSM559398     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559399     1  0.2345      0.827 0.900 0.000 0.00 0.100
#&gt; GSM559400     2  0.2345      0.891 0.000 0.900 0.00 0.100
#&gt; GSM559402     1  0.4382      0.665 0.704 0.000 0.00 0.296
#&gt; GSM559403     1  0.0188      0.849 0.996 0.000 0.00 0.004
#&gt; GSM559404     1  0.2760      0.837 0.872 0.000 0.00 0.128
#&gt; GSM559405     1  0.2530      0.842 0.888 0.000 0.00 0.112
#&gt; GSM559406     4  0.2216      0.936 0.092 0.000 0.00 0.908
#&gt; GSM559407     1  0.4585      0.605 0.668 0.000 0.00 0.332
#&gt; GSM559408     4  0.2345      0.937 0.100 0.000 0.00 0.900
#&gt; GSM559409     4  0.2469      0.933 0.108 0.000 0.00 0.892
#&gt; GSM559410     1  0.2345      0.847 0.900 0.000 0.00 0.100
#&gt; GSM559411     4  0.2408      0.937 0.104 0.000 0.00 0.896
#&gt; GSM559412     4  0.2408      0.937 0.104 0.000 0.00 0.896
#&gt; GSM559413     4  0.2408      0.937 0.104 0.000 0.00 0.896
#&gt; GSM559415     1  0.2345      0.810 0.900 0.000 0.00 0.100
#&gt; GSM559416     4  0.0592      0.901 0.016 0.000 0.00 0.984
#&gt; GSM559417     4  0.0779      0.898 0.016 0.004 0.00 0.980
#&gt; GSM559418     1  0.2924      0.801 0.884 0.016 0.00 0.100
#&gt; GSM559419     4  0.0817      0.903 0.024 0.000 0.00 0.976
#&gt; GSM559420     1  0.4907      0.458 0.580 0.000 0.00 0.420
#&gt; GSM559421     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559423     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559425     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559426     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559427     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559428     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559429     2  0.0000      0.986 0.000 1.000 0.00 0.000
#&gt; GSM559430     2  0.0000      0.986 0.000 1.000 0.00 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.0898    0.91630 0.020 0.000 0.972 0.008 0.000
#&gt; GSM559424     3  0.0000    0.93187 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.0404    0.93341 0.012 0.988 0.000 0.000 0.000
#&gt; GSM559432     3  0.2899    0.81573 0.020 0.100 0.872 0.008 0.000
#&gt; GSM559381     1  0.4138    0.43833 0.708 0.000 0.000 0.016 0.276
#&gt; GSM559382     2  0.2599    0.89530 0.044 0.904 0.000 0.024 0.028
#&gt; GSM559384     1  0.3452    0.59517 0.820 0.000 0.000 0.032 0.148
#&gt; GSM559385     5  0.0880    0.77085 0.032 0.000 0.000 0.000 0.968
#&gt; GSM559386     2  0.6471    0.54140 0.084 0.612 0.000 0.076 0.228
#&gt; GSM559388     2  0.1173    0.92553 0.020 0.964 0.000 0.012 0.004
#&gt; GSM559389     5  0.4063    0.53087 0.280 0.000 0.000 0.012 0.708
#&gt; GSM559390     4  0.3848    0.60543 0.172 0.000 0.000 0.788 0.040
#&gt; GSM559392     2  0.0324    0.93570 0.004 0.992 0.000 0.004 0.000
#&gt; GSM559393     5  0.1041    0.75024 0.032 0.004 0.000 0.000 0.964
#&gt; GSM559394     5  0.0963    0.77309 0.036 0.000 0.000 0.000 0.964
#&gt; GSM559396     3  0.5328    0.16023 0.468 0.000 0.492 0.028 0.012
#&gt; GSM559398     2  0.0324    0.93570 0.004 0.992 0.000 0.004 0.000
#&gt; GSM559399     5  0.5166    0.69407 0.108 0.000 0.000 0.212 0.680
#&gt; GSM559400     2  0.4674    0.59481 0.024 0.676 0.000 0.292 0.008
#&gt; GSM559402     1  0.3192    0.61203 0.848 0.000 0.000 0.040 0.112
#&gt; GSM559403     5  0.1608    0.76137 0.072 0.000 0.000 0.000 0.928
#&gt; GSM559404     1  0.4622    0.56231 0.684 0.000 0.000 0.040 0.276
#&gt; GSM559405     1  0.4256    0.17987 0.564 0.000 0.000 0.000 0.436
#&gt; GSM559406     4  0.4626    0.37314 0.364 0.000 0.000 0.616 0.020
#&gt; GSM559407     1  0.3705    0.60676 0.816 0.000 0.000 0.064 0.120
#&gt; GSM559408     4  0.4829    0.04987 0.480 0.000 0.000 0.500 0.020
#&gt; GSM559409     1  0.4848    0.00951 0.556 0.000 0.000 0.420 0.024
#&gt; GSM559410     5  0.5043    0.29642 0.356 0.000 0.000 0.044 0.600
#&gt; GSM559411     1  0.3992    0.39913 0.720 0.000 0.000 0.268 0.012
#&gt; GSM559412     1  0.4744    0.08718 0.572 0.000 0.000 0.408 0.020
#&gt; GSM559413     1  0.4437    0.32171 0.664 0.000 0.000 0.316 0.020
#&gt; GSM559415     5  0.4342    0.71817 0.040 0.000 0.000 0.232 0.728
#&gt; GSM559416     4  0.0963    0.66264 0.036 0.000 0.000 0.964 0.000
#&gt; GSM559417     4  0.0963    0.66264 0.036 0.000 0.000 0.964 0.000
#&gt; GSM559418     5  0.4587    0.71954 0.040 0.008 0.000 0.228 0.724
#&gt; GSM559419     4  0.3409    0.60079 0.144 0.000 0.000 0.824 0.032
#&gt; GSM559420     1  0.4277    0.52844 0.768 0.000 0.000 0.156 0.076
#&gt; GSM559421     2  0.0162    0.93632 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559423     2  0.0000    0.93655 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.0000    0.93655 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0162    0.93625 0.004 0.996 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000    0.93655 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.2312    0.89764 0.060 0.912 0.000 0.012 0.016
#&gt; GSM559429     2  0.0703    0.93037 0.024 0.976 0.000 0.000 0.000
#&gt; GSM559430     2  0.0000    0.93655 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000     0.9008 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.0000     0.9008 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0000     0.9008 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559395     3  0.0000     0.9008 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0146     0.9002 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559401     3  0.0603     0.8939 0.000 0.000 0.980 0.000 0.004 0.016
#&gt; GSM559414     3  0.0146     0.9002 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559422     3  0.2331     0.8394 0.000 0.000 0.888 0.000 0.032 0.080
#&gt; GSM559424     3  0.0000     0.9008 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559431     2  0.0777     0.8650 0.000 0.972 0.000 0.000 0.004 0.024
#&gt; GSM559432     3  0.4357     0.7202 0.000 0.104 0.768 0.000 0.040 0.088
#&gt; GSM559381     6  0.6162     0.4505 0.208 0.008 0.000 0.216 0.020 0.548
#&gt; GSM559382     2  0.4776     0.7254 0.036 0.712 0.000 0.000 0.068 0.184
#&gt; GSM559384     6  0.6296     0.3114 0.112 0.000 0.000 0.416 0.052 0.420
#&gt; GSM559385     1  0.1245     0.5486 0.952 0.000 0.000 0.032 0.000 0.016
#&gt; GSM559386     2  0.7442     0.2179 0.156 0.376 0.000 0.020 0.100 0.348
#&gt; GSM559388     2  0.2803     0.8225 0.004 0.864 0.000 0.000 0.048 0.084
#&gt; GSM559389     1  0.5171    -0.0855 0.512 0.000 0.000 0.076 0.004 0.408
#&gt; GSM559390     5  0.6290     0.3795 0.032 0.000 0.000 0.332 0.472 0.164
#&gt; GSM559392     2  0.1088     0.8641 0.000 0.960 0.000 0.000 0.024 0.016
#&gt; GSM559393     1  0.2122     0.5059 0.900 0.000 0.000 0.000 0.024 0.076
#&gt; GSM559394     1  0.1218     0.5573 0.956 0.000 0.000 0.028 0.012 0.004
#&gt; GSM559396     3  0.7120    -0.2563 0.008 0.000 0.352 0.236 0.056 0.348
#&gt; GSM559398     2  0.0914     0.8661 0.000 0.968 0.000 0.000 0.016 0.016
#&gt; GSM559399     1  0.6584     0.3921 0.460 0.000 0.000 0.056 0.320 0.164
#&gt; GSM559400     2  0.5827     0.4348 0.008 0.552 0.000 0.012 0.300 0.128
#&gt; GSM559402     4  0.5516    -0.3115 0.080 0.000 0.000 0.584 0.032 0.304
#&gt; GSM559403     1  0.2309     0.5359 0.888 0.000 0.000 0.084 0.000 0.028
#&gt; GSM559404     4  0.5438    -0.0924 0.284 0.000 0.000 0.572 0.004 0.140
#&gt; GSM559405     1  0.6572    -0.3497 0.372 0.000 0.000 0.332 0.024 0.272
#&gt; GSM559406     4  0.4527     0.1761 0.012 0.000 0.000 0.664 0.284 0.040
#&gt; GSM559407     4  0.5421    -0.1219 0.088 0.000 0.000 0.636 0.040 0.236
#&gt; GSM559408     4  0.3420     0.3876 0.008 0.000 0.000 0.776 0.204 0.012
#&gt; GSM559409     4  0.3033     0.5010 0.012 0.000 0.000 0.848 0.108 0.032
#&gt; GSM559410     1  0.6041     0.1823 0.512 0.000 0.000 0.348 0.068 0.072
#&gt; GSM559411     4  0.1297     0.4896 0.000 0.000 0.000 0.948 0.012 0.040
#&gt; GSM559412     4  0.2400     0.5153 0.004 0.000 0.000 0.872 0.116 0.008
#&gt; GSM559413     4  0.0692     0.5237 0.000 0.000 0.000 0.976 0.020 0.004
#&gt; GSM559415     1  0.5218     0.4308 0.544 0.000 0.000 0.012 0.376 0.068
#&gt; GSM559416     5  0.3329     0.7382 0.004 0.000 0.000 0.236 0.756 0.004
#&gt; GSM559417     5  0.3543     0.7394 0.004 0.000 0.000 0.212 0.764 0.020
#&gt; GSM559418     1  0.5269     0.4174 0.532 0.008 0.000 0.000 0.380 0.080
#&gt; GSM559419     5  0.4988     0.6208 0.024 0.000 0.000 0.224 0.672 0.080
#&gt; GSM559420     4  0.6570    -0.4708 0.052 0.000 0.000 0.400 0.156 0.392
#&gt; GSM559421     2  0.0508     0.8688 0.000 0.984 0.000 0.000 0.004 0.012
#&gt; GSM559423     2  0.0260     0.8696 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559425     2  0.0146     0.8693 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559426     2  0.0806     0.8668 0.000 0.972 0.000 0.000 0.008 0.020
#&gt; GSM559427     2  0.0000     0.8697 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.4312     0.7237 0.008 0.724 0.000 0.000 0.064 0.204
#&gt; GSM559429     2  0.2679     0.8178 0.000 0.864 0.000 0.000 0.040 0.096
#&gt; GSM559430     2  0.0000     0.8697 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> CV:skmeans 52         6.10e-01 2
#> CV:skmeans 51         1.98e-09 3
#> CV:skmeans 51         1.01e-08 4
#> CV:skmeans 42         1.66e-07 5
#> CV:skmeans 33         8.40e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.975       0.974         0.3202 0.683   0.683
#> 3 3 0.999           0.962       0.985         0.7828 0.743   0.624
#> 4 4 0.832           0.895       0.944         0.2335 0.860   0.677
#> 5 5 0.825           0.886       0.934         0.0233 0.988   0.959
#> 6 6 0.850           0.830       0.917         0.0230 0.993   0.976
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2   0.358      0.992 0.068 0.932
#&gt; GSM559387     2   0.358      0.992 0.068 0.932
#&gt; GSM559391     2   0.358      0.992 0.068 0.932
#&gt; GSM559395     2   0.358      0.992 0.068 0.932
#&gt; GSM559397     2   0.358      0.992 0.068 0.932
#&gt; GSM559401     2   0.358      0.992 0.068 0.932
#&gt; GSM559414     2   0.358      0.992 0.068 0.932
#&gt; GSM559422     2   0.358      0.992 0.068 0.932
#&gt; GSM559424     2   0.358      0.992 0.068 0.932
#&gt; GSM559431     1   0.358      0.945 0.932 0.068
#&gt; GSM559432     2   0.000      0.929 0.000 1.000
#&gt; GSM559381     1   0.000      0.981 1.000 0.000
#&gt; GSM559382     1   0.000      0.981 1.000 0.000
#&gt; GSM559384     1   0.000      0.981 1.000 0.000
#&gt; GSM559385     1   0.000      0.981 1.000 0.000
#&gt; GSM559386     1   0.000      0.981 1.000 0.000
#&gt; GSM559388     1   0.118      0.973 0.984 0.016
#&gt; GSM559389     1   0.000      0.981 1.000 0.000
#&gt; GSM559390     1   0.000      0.981 1.000 0.000
#&gt; GSM559392     1   0.358      0.945 0.932 0.068
#&gt; GSM559393     1   0.000      0.981 1.000 0.000
#&gt; GSM559394     1   0.000      0.981 1.000 0.000
#&gt; GSM559396     1   0.000      0.981 1.000 0.000
#&gt; GSM559398     1   0.358      0.945 0.932 0.068
#&gt; GSM559399     1   0.000      0.981 1.000 0.000
#&gt; GSM559400     1   0.311      0.952 0.944 0.056
#&gt; GSM559402     1   0.000      0.981 1.000 0.000
#&gt; GSM559403     1   0.000      0.981 1.000 0.000
#&gt; GSM559404     1   0.000      0.981 1.000 0.000
#&gt; GSM559405     1   0.000      0.981 1.000 0.000
#&gt; GSM559406     1   0.000      0.981 1.000 0.000
#&gt; GSM559407     1   0.000      0.981 1.000 0.000
#&gt; GSM559408     1   0.000      0.981 1.000 0.000
#&gt; GSM559409     1   0.000      0.981 1.000 0.000
#&gt; GSM559410     1   0.000      0.981 1.000 0.000
#&gt; GSM559411     1   0.000      0.981 1.000 0.000
#&gt; GSM559412     1   0.000      0.981 1.000 0.000
#&gt; GSM559413     1   0.000      0.981 1.000 0.000
#&gt; GSM559415     1   0.000      0.981 1.000 0.000
#&gt; GSM559416     1   0.000      0.981 1.000 0.000
#&gt; GSM559417     1   0.000      0.981 1.000 0.000
#&gt; GSM559418     1   0.000      0.981 1.000 0.000
#&gt; GSM559419     1   0.000      0.981 1.000 0.000
#&gt; GSM559420     1   0.000      0.981 1.000 0.000
#&gt; GSM559421     1   0.358      0.945 0.932 0.068
#&gt; GSM559423     1   0.358      0.945 0.932 0.068
#&gt; GSM559425     1   0.358      0.945 0.932 0.068
#&gt; GSM559426     1   0.358      0.945 0.932 0.068
#&gt; GSM559427     1   0.358      0.945 0.932 0.068
#&gt; GSM559428     1   0.000      0.981 1.000 0.000
#&gt; GSM559429     1   0.295      0.954 0.948 0.052
#&gt; GSM559430     1   0.358      0.945 0.932 0.068
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3
#&gt; GSM559383     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559387     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559391     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559395     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559397     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559401     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559414     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559422     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559424     3  0.0000      0.979 0.000 0.000 1.00
#&gt; GSM559431     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559432     3  0.4291      0.775 0.000 0.180 0.82
#&gt; GSM559381     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559382     1  0.0747      0.977 0.984 0.016 0.00
#&gt; GSM559384     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559385     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559386     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559388     1  0.4399      0.760 0.812 0.188 0.00
#&gt; GSM559389     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559390     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559392     2  0.1529      0.909 0.040 0.960 0.00
#&gt; GSM559393     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559394     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559396     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559398     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559399     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559400     2  0.5431      0.578 0.284 0.716 0.00
#&gt; GSM559402     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559403     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559404     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559405     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559406     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559407     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559408     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559409     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559410     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559411     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559412     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559413     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559415     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559416     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559417     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559418     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559419     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559420     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559421     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559423     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559425     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559426     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559427     2  0.0000      0.942 0.000 1.000 0.00
#&gt; GSM559428     1  0.0000      0.993 1.000 0.000 0.00
#&gt; GSM559429     2  0.2261      0.876 0.068 0.932 0.00
#&gt; GSM559430     2  0.0000      0.942 0.000 1.000 0.00
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3    p4
#&gt; GSM559383     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559387     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559391     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559395     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559397     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559401     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559414     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559422     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559424     3  0.0000      0.978 0.000 0.000 1.00 0.000
#&gt; GSM559431     2  0.0000      0.977 0.000 1.000 0.00 0.000
#&gt; GSM559432     3  0.3400      0.778 0.000 0.180 0.82 0.000
#&gt; GSM559381     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559382     4  0.3311      0.811 0.172 0.000 0.00 0.828
#&gt; GSM559384     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559385     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559386     4  0.4543      0.695 0.324 0.000 0.00 0.676
#&gt; GSM559388     4  0.5321      0.716 0.296 0.032 0.00 0.672
#&gt; GSM559389     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559390     4  0.0336      0.808 0.008 0.000 0.00 0.992
#&gt; GSM559392     2  0.2892      0.878 0.068 0.896 0.00 0.036
#&gt; GSM559393     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559394     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559396     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559398     2  0.0336      0.975 0.000 0.992 0.00 0.008
#&gt; GSM559399     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559400     4  0.3219      0.809 0.164 0.000 0.00 0.836
#&gt; GSM559402     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559403     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559404     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559405     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559406     4  0.1302      0.822 0.044 0.000 0.00 0.956
#&gt; GSM559407     1  0.1637      0.893 0.940 0.000 0.00 0.060
#&gt; GSM559408     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559409     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559410     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559411     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559412     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559413     1  0.3219      0.823 0.836 0.000 0.00 0.164
#&gt; GSM559415     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559416     4  0.1118      0.822 0.036 0.000 0.00 0.964
#&gt; GSM559417     4  0.1302      0.825 0.044 0.000 0.00 0.956
#&gt; GSM559418     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559419     1  0.1716      0.875 0.936 0.000 0.00 0.064
#&gt; GSM559420     1  0.0000      0.924 1.000 0.000 0.00 0.000
#&gt; GSM559421     2  0.1118      0.960 0.000 0.964 0.00 0.036
#&gt; GSM559423     2  0.1042      0.960 0.020 0.972 0.00 0.008
#&gt; GSM559425     2  0.0000      0.977 0.000 1.000 0.00 0.000
#&gt; GSM559426     2  0.0000      0.977 0.000 1.000 0.00 0.000
#&gt; GSM559427     2  0.0000      0.977 0.000 1.000 0.00 0.000
#&gt; GSM559428     1  0.4382      0.444 0.704 0.000 0.00 0.296
#&gt; GSM559429     2  0.0336      0.974 0.008 0.992 0.00 0.000
#&gt; GSM559430     2  0.0000      0.977 0.000 1.000 0.00 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3    p4    p5
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559422     5  0.2732      1.000 0.000 0.000 0.16 0.000 0.840
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.00 0.000 0.000
#&gt; GSM559431     2  0.0000      0.942 0.000 1.000 0.00 0.000 0.000
#&gt; GSM559432     5  0.2732      1.000 0.000 0.000 0.16 0.000 0.840
#&gt; GSM559381     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559382     4  0.3093      0.756 0.168 0.000 0.00 0.824 0.008
#&gt; GSM559384     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559385     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559386     4  0.4366      0.615 0.320 0.000 0.00 0.664 0.016
#&gt; GSM559388     4  0.5996      0.657 0.192 0.024 0.00 0.644 0.140
#&gt; GSM559389     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559390     4  0.0162      0.757 0.004 0.000 0.00 0.996 0.000
#&gt; GSM559392     2  0.3521      0.856 0.008 0.824 0.00 0.024 0.144
#&gt; GSM559393     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559394     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559396     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559398     2  0.2439      0.888 0.000 0.876 0.00 0.004 0.120
#&gt; GSM559399     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559400     4  0.2773      0.756 0.164 0.000 0.00 0.836 0.000
#&gt; GSM559402     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559403     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559404     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559405     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559406     4  0.0880      0.768 0.032 0.000 0.00 0.968 0.000
#&gt; GSM559407     1  0.1410      0.895 0.940 0.000 0.00 0.060 0.000
#&gt; GSM559408     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559409     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559410     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559411     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559412     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559413     1  0.2773      0.827 0.836 0.000 0.00 0.164 0.000
#&gt; GSM559415     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559416     4  0.0703      0.769 0.024 0.000 0.00 0.976 0.000
#&gt; GSM559417     4  0.0880      0.773 0.032 0.000 0.00 0.968 0.000
#&gt; GSM559418     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559419     1  0.1845      0.870 0.928 0.000 0.00 0.056 0.016
#&gt; GSM559420     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM559421     2  0.3241      0.863 0.000 0.832 0.00 0.024 0.144
#&gt; GSM559423     2  0.1082      0.934 0.008 0.964 0.00 0.000 0.028
#&gt; GSM559425     2  0.0000      0.942 0.000 1.000 0.00 0.000 0.000
#&gt; GSM559426     2  0.0510      0.938 0.000 0.984 0.00 0.000 0.016
#&gt; GSM559427     2  0.0000      0.942 0.000 1.000 0.00 0.000 0.000
#&gt; GSM559428     1  0.4206      0.444 0.696 0.000 0.00 0.288 0.016
#&gt; GSM559429     2  0.0798      0.935 0.008 0.976 0.00 0.000 0.016
#&gt; GSM559430     2  0.0290      0.941 0.000 0.992 0.00 0.000 0.008
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3    p4 p5    p6
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559422     5  0.0000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000  1 0.000  0 0.000
#&gt; GSM559431     2  0.0146      0.796 0.000 0.996  0 0.000  0 0.004
#&gt; GSM559432     5  0.0000      1.000 0.000 0.000  0 0.000  1 0.000
#&gt; GSM559381     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559382     4  0.3469      0.716 0.088 0.000  0 0.808  0 0.104
#&gt; GSM559384     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559385     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559386     4  0.4892      0.548 0.248 0.000  0 0.640  0 0.112
#&gt; GSM559388     4  0.5712      0.388 0.096 0.020  0 0.468  0 0.416
#&gt; GSM559389     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559390     4  0.0000      0.757 0.000 0.000  0 1.000  0 0.000
#&gt; GSM559392     2  0.3684      0.603 0.000 0.628  0 0.000  0 0.372
#&gt; GSM559393     1  0.1204      0.898 0.944 0.000  0 0.000  0 0.056
#&gt; GSM559394     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559396     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559398     2  0.2730      0.722 0.000 0.808  0 0.000  0 0.192
#&gt; GSM559399     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559400     4  0.3327      0.719 0.088 0.000  0 0.820  0 0.092
#&gt; GSM559402     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559403     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559404     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559405     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559406     4  0.0260      0.757 0.008 0.000  0 0.992  0 0.000
#&gt; GSM559407     1  0.0632      0.919 0.976 0.000  0 0.024  0 0.000
#&gt; GSM559408     1  0.2631      0.822 0.820 0.000  0 0.180  0 0.000
#&gt; GSM559409     1  0.2631      0.822 0.820 0.000  0 0.180  0 0.000
#&gt; GSM559410     1  0.2454      0.838 0.840 0.000  0 0.160  0 0.000
#&gt; GSM559411     1  0.1765      0.878 0.904 0.000  0 0.096  0 0.000
#&gt; GSM559412     1  0.2631      0.822 0.820 0.000  0 0.180  0 0.000
#&gt; GSM559413     1  0.2631      0.822 0.820 0.000  0 0.180  0 0.000
#&gt; GSM559415     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559416     4  0.0000      0.757 0.000 0.000  0 1.000  0 0.000
#&gt; GSM559417     4  0.0632      0.754 0.024 0.000  0 0.976  0 0.000
#&gt; GSM559418     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559419     1  0.1549      0.891 0.936 0.000  0 0.044  0 0.020
#&gt; GSM559420     1  0.0000      0.929 1.000 0.000  0 0.000  0 0.000
#&gt; GSM559421     2  0.3547      0.649 0.000 0.668  0 0.000  0 0.332
#&gt; GSM559423     2  0.2416      0.763 0.000 0.844  0 0.000  0 0.156
#&gt; GSM559425     2  0.0000      0.798 0.000 1.000  0 0.000  0 0.000
#&gt; GSM559426     2  0.1610      0.779 0.000 0.916  0 0.000  0 0.084
#&gt; GSM559427     2  0.0000      0.798 0.000 1.000  0 0.000  0 0.000
#&gt; GSM559428     1  0.5048      0.344 0.616 0.000  0 0.264  0 0.120
#&gt; GSM559429     6  0.3547      0.000 0.000 0.332  0 0.000  0 0.668
#&gt; GSM559430     2  0.0547      0.802 0.000 0.980  0 0.000  0 0.020
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:pam 52         1.99e-10 2
#> CV:pam 52         7.80e-11 3
#> CV:pam 51         6.63e-10 4
#> CV:pam 51         2.87e-09 5
#> CV:pam 49         6.75e-09 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.976       0.986         0.3306 0.683   0.683
#> 3 3 0.628           0.842       0.908         0.8546 0.716   0.584
#> 4 4 0.699           0.548       0.805         0.1460 0.925   0.811
#> 5 5 0.683           0.670       0.801         0.0472 0.851   0.607
#> 6 6 0.730           0.633       0.787         0.0484 0.948   0.830
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.0000      1.000 0.000 1.000
#&gt; GSM559387     2  0.0000      1.000 0.000 1.000
#&gt; GSM559391     2  0.0000      1.000 0.000 1.000
#&gt; GSM559395     2  0.0000      1.000 0.000 1.000
#&gt; GSM559397     2  0.0000      1.000 0.000 1.000
#&gt; GSM559401     2  0.0000      1.000 0.000 1.000
#&gt; GSM559414     2  0.0000      1.000 0.000 1.000
#&gt; GSM559422     2  0.0000      1.000 0.000 1.000
#&gt; GSM559424     2  0.0000      1.000 0.000 1.000
#&gt; GSM559431     1  0.6148      0.844 0.848 0.152
#&gt; GSM559432     2  0.0000      1.000 0.000 1.000
#&gt; GSM559381     1  0.0000      0.982 1.000 0.000
#&gt; GSM559382     1  0.1414      0.976 0.980 0.020
#&gt; GSM559384     1  0.0000      0.982 1.000 0.000
#&gt; GSM559385     1  0.0376      0.981 0.996 0.004
#&gt; GSM559386     1  0.0000      0.982 1.000 0.000
#&gt; GSM559388     1  0.1414      0.976 0.980 0.020
#&gt; GSM559389     1  0.0000      0.982 1.000 0.000
#&gt; GSM559390     1  0.0000      0.982 1.000 0.000
#&gt; GSM559392     1  0.1414      0.976 0.980 0.020
#&gt; GSM559393     1  0.0376      0.981 0.996 0.004
#&gt; GSM559394     1  0.0376      0.981 0.996 0.004
#&gt; GSM559396     1  0.2778      0.956 0.952 0.048
#&gt; GSM559398     1  0.1414      0.976 0.980 0.020
#&gt; GSM559399     1  0.0000      0.982 1.000 0.000
#&gt; GSM559400     1  0.1414      0.976 0.980 0.020
#&gt; GSM559402     1  0.0000      0.982 1.000 0.000
#&gt; GSM559403     1  0.0376      0.981 0.996 0.004
#&gt; GSM559404     1  0.0376      0.981 0.996 0.004
#&gt; GSM559405     1  0.0000      0.982 1.000 0.000
#&gt; GSM559406     1  0.0000      0.982 1.000 0.000
#&gt; GSM559407     1  0.0000      0.982 1.000 0.000
#&gt; GSM559408     1  0.0000      0.982 1.000 0.000
#&gt; GSM559409     1  0.0000      0.982 1.000 0.000
#&gt; GSM559410     1  0.0000      0.982 1.000 0.000
#&gt; GSM559411     1  0.0000      0.982 1.000 0.000
#&gt; GSM559412     1  0.0000      0.982 1.000 0.000
#&gt; GSM559413     1  0.0000      0.982 1.000 0.000
#&gt; GSM559415     1  0.0000      0.982 1.000 0.000
#&gt; GSM559416     1  0.0376      0.981 0.996 0.004
#&gt; GSM559417     1  0.0000      0.982 1.000 0.000
#&gt; GSM559418     1  0.0000      0.982 1.000 0.000
#&gt; GSM559419     1  0.0000      0.982 1.000 0.000
#&gt; GSM559420     1  0.0000      0.982 1.000 0.000
#&gt; GSM559421     1  0.1414      0.976 0.980 0.020
#&gt; GSM559423     1  0.1414      0.976 0.980 0.020
#&gt; GSM559425     1  0.1414      0.976 0.980 0.020
#&gt; GSM559426     1  0.1414      0.976 0.980 0.020
#&gt; GSM559427     1  0.1414      0.976 0.980 0.020
#&gt; GSM559428     1  0.6148      0.844 0.848 0.152
#&gt; GSM559429     1  0.6148      0.844 0.848 0.152
#&gt; GSM559430     1  0.1414      0.976 0.980 0.020
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2   p3
#&gt; GSM559383     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559387     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559391     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559395     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559397     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559401     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559414     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559422     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559424     3  0.0000      0.998 0.000 0.000 1.00
#&gt; GSM559431     2  0.0424      0.908 0.008 0.992 0.00
#&gt; GSM559432     3  0.0892      0.979 0.000 0.020 0.98
#&gt; GSM559381     1  0.2796      0.846 0.908 0.092 0.00
#&gt; GSM559382     2  0.3192      0.866 0.112 0.888 0.00
#&gt; GSM559384     1  0.2878      0.847 0.904 0.096 0.00
#&gt; GSM559385     1  0.4504      0.690 0.804 0.196 0.00
#&gt; GSM559386     1  0.6095      0.468 0.608 0.392 0.00
#&gt; GSM559388     2  0.1529      0.934 0.040 0.960 0.00
#&gt; GSM559389     1  0.0237      0.845 0.996 0.004 0.00
#&gt; GSM559390     1  0.4399      0.805 0.812 0.188 0.00
#&gt; GSM559392     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559393     1  0.4452      0.691 0.808 0.192 0.00
#&gt; GSM559394     1  0.4504      0.690 0.804 0.196 0.00
#&gt; GSM559396     1  0.5785      0.646 0.668 0.332 0.00
#&gt; GSM559398     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559399     1  0.2711      0.847 0.912 0.088 0.00
#&gt; GSM559400     1  0.6225      0.403 0.568 0.432 0.00
#&gt; GSM559402     1  0.0747      0.848 0.984 0.016 0.00
#&gt; GSM559403     1  0.0424      0.844 0.992 0.008 0.00
#&gt; GSM559404     1  0.3340      0.801 0.880 0.120 0.00
#&gt; GSM559405     1  0.0237      0.845 0.996 0.004 0.00
#&gt; GSM559406     1  0.4555      0.796 0.800 0.200 0.00
#&gt; GSM559407     1  0.0237      0.845 0.996 0.004 0.00
#&gt; GSM559408     1  0.0424      0.848 0.992 0.008 0.00
#&gt; GSM559409     1  0.1964      0.851 0.944 0.056 0.00
#&gt; GSM559410     1  0.0237      0.845 0.996 0.004 0.00
#&gt; GSM559411     1  0.4452      0.802 0.808 0.192 0.00
#&gt; GSM559412     1  0.4002      0.812 0.840 0.160 0.00
#&gt; GSM559413     1  0.4605      0.795 0.796 0.204 0.00
#&gt; GSM559415     1  0.1031      0.845 0.976 0.024 0.00
#&gt; GSM559416     1  0.0000      0.845 1.000 0.000 0.00
#&gt; GSM559417     1  0.2878      0.846 0.904 0.096 0.00
#&gt; GSM559418     1  0.6140      0.436 0.596 0.404 0.00
#&gt; GSM559419     1  0.0424      0.848 0.992 0.008 0.00
#&gt; GSM559420     1  0.3816      0.820 0.852 0.148 0.00
#&gt; GSM559421     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559423     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559425     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559426     2  0.1289      0.933 0.032 0.968 0.00
#&gt; GSM559427     2  0.1411      0.936 0.036 0.964 0.00
#&gt; GSM559428     2  0.5016      0.626 0.240 0.760 0.00
#&gt; GSM559429     2  0.4750      0.664 0.216 0.784 0.00
#&gt; GSM559430     2  0.1411      0.936 0.036 0.964 0.00
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000      0.941 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000      0.941 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0469      0.936 0.000 0.000 0.988 0.012
#&gt; GSM559395     3  0.0000      0.941 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000      0.941 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0707      0.936 0.000 0.000 0.980 0.020
#&gt; GSM559414     3  0.0000      0.941 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.4431      0.798 0.000 0.000 0.696 0.304
#&gt; GSM559424     3  0.2011      0.885 0.000 0.000 0.920 0.080
#&gt; GSM559431     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559432     3  0.4431      0.798 0.000 0.000 0.696 0.304
#&gt; GSM559381     1  0.0469      0.616 0.988 0.000 0.000 0.012
#&gt; GSM559382     2  0.2125      0.875 0.004 0.920 0.000 0.076
#&gt; GSM559384     1  0.1022      0.616 0.968 0.000 0.000 0.032
#&gt; GSM559385     1  0.3266      0.552 0.832 0.000 0.000 0.168
#&gt; GSM559386     1  0.2197      0.589 0.916 0.080 0.000 0.004
#&gt; GSM559388     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559389     1  0.0779      0.615 0.980 0.004 0.000 0.016
#&gt; GSM559390     4  0.4998      0.438 0.488 0.000 0.000 0.512
#&gt; GSM559392     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.3266      0.552 0.832 0.000 0.000 0.168
#&gt; GSM559394     1  0.3266      0.552 0.832 0.000 0.000 0.168
#&gt; GSM559396     1  0.7100     -0.149 0.512 0.084 0.016 0.388
#&gt; GSM559398     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.2081      0.602 0.916 0.000 0.000 0.084
#&gt; GSM559400     4  0.5764     -0.181 0.028 0.452 0.000 0.520
#&gt; GSM559402     1  0.2011      0.604 0.920 0.000 0.000 0.080
#&gt; GSM559403     1  0.2281      0.592 0.904 0.000 0.000 0.096
#&gt; GSM559404     1  0.3486      0.539 0.812 0.000 0.000 0.188
#&gt; GSM559405     1  0.0779      0.615 0.980 0.004 0.000 0.016
#&gt; GSM559406     4  0.4998      0.438 0.488 0.000 0.000 0.512
#&gt; GSM559407     1  0.2149      0.600 0.912 0.000 0.000 0.088
#&gt; GSM559408     1  0.4955     -0.367 0.556 0.000 0.000 0.444
#&gt; GSM559409     1  0.4790     -0.210 0.620 0.000 0.000 0.380
#&gt; GSM559410     1  0.2266      0.602 0.912 0.004 0.000 0.084
#&gt; GSM559411     4  0.4998      0.438 0.488 0.000 0.000 0.512
#&gt; GSM559412     1  0.4961     -0.389 0.552 0.000 0.000 0.448
#&gt; GSM559413     1  0.4994     -0.505 0.520 0.000 0.000 0.480
#&gt; GSM559415     1  0.1978      0.609 0.928 0.004 0.000 0.068
#&gt; GSM559416     1  0.4955     -0.337 0.556 0.000 0.000 0.444
#&gt; GSM559417     1  0.4955     -0.337 0.556 0.000 0.000 0.444
#&gt; GSM559418     1  0.2542      0.585 0.904 0.084 0.000 0.012
#&gt; GSM559419     1  0.4955     -0.337 0.556 0.000 0.000 0.444
#&gt; GSM559420     1  0.4477      0.160 0.688 0.000 0.000 0.312
#&gt; GSM559421     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.7002      0.339 0.164 0.568 0.000 0.268
#&gt; GSM559429     2  0.4370      0.739 0.156 0.800 0.000 0.044
#&gt; GSM559430     2  0.0000      0.938 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.2561     0.8196 0.000 0.000 0.856 0.000 0.144
#&gt; GSM559414     3  0.0000     0.9764 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     5  0.5645    -0.0982 0.000 0.000 0.376 0.084 0.540
#&gt; GSM559424     3  0.0162     0.9738 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559431     2  0.3174     0.8610 0.004 0.844 0.020 0.132 0.000
#&gt; GSM559432     5  0.5645    -0.0982 0.000 0.000 0.376 0.084 0.540
#&gt; GSM559381     1  0.4403     0.5500 0.560 0.004 0.000 0.000 0.436
#&gt; GSM559382     2  0.4078     0.8050 0.096 0.816 0.000 0.064 0.024
#&gt; GSM559384     1  0.4415     0.5466 0.552 0.004 0.000 0.000 0.444
#&gt; GSM559385     4  0.6172     0.9703 0.176 0.000 0.000 0.544 0.280
#&gt; GSM559386     1  0.6932     0.3911 0.480 0.064 0.000 0.092 0.364
#&gt; GSM559388     2  0.0000     0.8913 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559389     1  0.6038     0.3341 0.448 0.004 0.000 0.100 0.448
#&gt; GSM559390     1  0.2753     0.5558 0.856 0.000 0.000 0.136 0.008
#&gt; GSM559392     2  0.0000     0.8913 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559393     4  0.6248     0.9518 0.176 0.000 0.000 0.524 0.300
#&gt; GSM559394     4  0.6172     0.9703 0.176 0.000 0.000 0.544 0.280
#&gt; GSM559396     1  0.4610     0.4931 0.760 0.008 0.000 0.140 0.092
#&gt; GSM559398     2  0.1544     0.8853 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559399     1  0.4273     0.5452 0.552 0.000 0.000 0.000 0.448
#&gt; GSM559400     2  0.6488     0.4816 0.272 0.516 0.000 0.208 0.004
#&gt; GSM559402     1  0.4403     0.5500 0.560 0.004 0.000 0.000 0.436
#&gt; GSM559403     5  0.6145    -0.6789 0.436 0.004 0.000 0.112 0.448
#&gt; GSM559404     4  0.6066     0.9338 0.188 0.000 0.000 0.572 0.240
#&gt; GSM559405     1  0.4425     0.5393 0.544 0.004 0.000 0.000 0.452
#&gt; GSM559406     1  0.1341     0.5634 0.944 0.000 0.000 0.056 0.000
#&gt; GSM559407     1  0.4201     0.5618 0.592 0.000 0.000 0.000 0.408
#&gt; GSM559408     1  0.1544     0.6212 0.932 0.000 0.000 0.000 0.068
#&gt; GSM559409     1  0.1792     0.6228 0.916 0.000 0.000 0.000 0.084
#&gt; GSM559410     1  0.4273     0.5452 0.552 0.000 0.000 0.000 0.448
#&gt; GSM559411     1  0.0451     0.5942 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559412     1  0.1571     0.6192 0.936 0.000 0.000 0.004 0.060
#&gt; GSM559413     1  0.1106     0.6069 0.964 0.000 0.000 0.012 0.024
#&gt; GSM559415     1  0.4425     0.5393 0.544 0.004 0.000 0.000 0.452
#&gt; GSM559416     1  0.2798     0.5032 0.852 0.000 0.000 0.140 0.008
#&gt; GSM559417     1  0.2136     0.5691 0.904 0.000 0.000 0.088 0.008
#&gt; GSM559418     1  0.4425     0.5393 0.544 0.004 0.000 0.000 0.452
#&gt; GSM559419     1  0.0290     0.5985 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559420     1  0.3109     0.5941 0.800 0.000 0.000 0.000 0.200
#&gt; GSM559421     2  0.0404     0.8916 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559423     2  0.0000     0.8913 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.1544     0.8853 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559426     2  0.0566     0.8891 0.000 0.984 0.000 0.012 0.004
#&gt; GSM559427     2  0.1544     0.8853 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559428     2  0.5887     0.6786 0.164 0.668 0.000 0.136 0.032
#&gt; GSM559429     2  0.3351     0.8308 0.028 0.836 0.000 0.132 0.004
#&gt; GSM559430     2  0.1544     0.8853 0.000 0.932 0.000 0.068 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM559383     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM559387     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM559391     3  0.1556      0.902 0.000 0.000 0.920 0.000 0.000 NA
#&gt; GSM559395     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM559397     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM559401     3  0.4614      0.617 0.000 0.000 0.684 0.000 0.208 NA
#&gt; GSM559414     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM559422     5  0.6212     -0.129 0.000 0.000 0.292 0.012 0.456 NA
#&gt; GSM559424     3  0.1556      0.902 0.000 0.000 0.920 0.000 0.000 NA
#&gt; GSM559431     2  0.4110      0.737 0.000 0.608 0.000 0.016 0.000 NA
#&gt; GSM559432     5  0.6216     -0.124 0.000 0.000 0.288 0.012 0.456 NA
#&gt; GSM559381     1  0.0000      0.695 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM559382     2  0.1554      0.823 0.004 0.940 0.000 0.004 0.044 NA
#&gt; GSM559384     1  0.0551      0.694 0.984 0.000 0.000 0.004 0.004 NA
#&gt; GSM559385     4  0.2793      0.983 0.200 0.000 0.000 0.800 0.000 NA
#&gt; GSM559386     1  0.2402      0.603 0.856 0.140 0.000 0.000 0.004 NA
#&gt; GSM559388     2  0.0000      0.836 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559389     1  0.0000      0.695 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM559390     1  0.7260      0.166 0.404 0.000 0.000 0.116 0.248 NA
#&gt; GSM559392     2  0.0000      0.836 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559393     4  0.2823      0.980 0.204 0.000 0.000 0.796 0.000 NA
#&gt; GSM559394     4  0.2793      0.983 0.200 0.000 0.000 0.800 0.000 NA
#&gt; GSM559396     5  0.6564     -0.200 0.320 0.000 0.000 0.032 0.420 NA
#&gt; GSM559398     2  0.3198      0.782 0.000 0.740 0.000 0.000 0.000 NA
#&gt; GSM559399     1  0.0405      0.694 0.988 0.000 0.000 0.004 0.008 NA
#&gt; GSM559400     2  0.6225      0.472 0.000 0.576 0.000 0.152 0.072 NA
#&gt; GSM559402     1  0.0363      0.695 0.988 0.000 0.000 0.000 0.012 NA
#&gt; GSM559403     1  0.2048      0.602 0.880 0.000 0.000 0.120 0.000 NA
#&gt; GSM559404     4  0.2562      0.952 0.172 0.000 0.000 0.828 0.000 NA
#&gt; GSM559405     1  0.0146      0.694 0.996 0.000 0.000 0.004 0.000 NA
#&gt; GSM559406     1  0.4165      0.412 0.536 0.000 0.000 0.000 0.452 NA
#&gt; GSM559407     1  0.1267      0.684 0.940 0.000 0.000 0.000 0.060 NA
#&gt; GSM559408     1  0.4118      0.487 0.592 0.000 0.000 0.004 0.396 NA
#&gt; GSM559409     1  0.4100      0.489 0.600 0.000 0.000 0.004 0.388 NA
#&gt; GSM559410     1  0.0405      0.694 0.988 0.000 0.000 0.004 0.008 NA
#&gt; GSM559411     5  0.3998     -0.519 0.492 0.000 0.000 0.000 0.504 NA
#&gt; GSM559412     1  0.4127      0.486 0.588 0.000 0.000 0.004 0.400 NA
#&gt; GSM559413     1  0.3789      0.473 0.584 0.000 0.000 0.000 0.416 NA
#&gt; GSM559415     1  0.0146      0.694 0.996 0.000 0.000 0.004 0.000 NA
#&gt; GSM559416     1  0.6139      0.171 0.384 0.000 0.000 0.004 0.372 NA
#&gt; GSM559417     1  0.6208      0.223 0.416 0.000 0.000 0.012 0.364 NA
#&gt; GSM559418     1  0.0260      0.694 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM559419     1  0.4124      0.369 0.516 0.000 0.000 0.004 0.476 NA
#&gt; GSM559420     1  0.3290      0.589 0.744 0.000 0.000 0.004 0.252 NA
#&gt; GSM559421     2  0.0632      0.838 0.000 0.976 0.000 0.000 0.000 NA
#&gt; GSM559423     2  0.0000      0.836 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559425     2  0.3330      0.774 0.000 0.716 0.000 0.000 0.000 NA
#&gt; GSM559426     2  0.1003      0.833 0.020 0.964 0.000 0.000 0.000 NA
#&gt; GSM559427     2  0.3330      0.774 0.000 0.716 0.000 0.000 0.000 NA
#&gt; GSM559428     2  0.3277      0.782 0.008 0.836 0.000 0.020 0.016 NA
#&gt; GSM559429     2  0.2745      0.799 0.000 0.860 0.000 0.020 0.008 NA
#&gt; GSM559430     2  0.3266      0.780 0.000 0.728 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:mclust 52         1.99e-10 2
#> CV:mclust 49         3.24e-10 3
#> CV:mclust 38         5.20e-08 4
#> CV:mclust 45         1.55e-08 5
#> CV:mclust 38         3.39e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.418           0.820       0.861         0.4504 0.527   0.527
#> 3 3 1.000           0.973       0.988         0.3671 0.702   0.509
#> 4 4 0.707           0.703       0.854         0.1689 0.867   0.665
#> 5 5 0.702           0.637       0.817         0.0822 0.845   0.524
#> 6 6 0.740           0.657       0.799         0.0565 0.870   0.514
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.7815      0.756 0.768 0.232
#&gt; GSM559387     1  0.7815      0.756 0.768 0.232
#&gt; GSM559391     1  0.7815      0.756 0.768 0.232
#&gt; GSM559395     1  0.7815      0.756 0.768 0.232
#&gt; GSM559397     1  0.7815      0.756 0.768 0.232
#&gt; GSM559401     1  0.7815      0.756 0.768 0.232
#&gt; GSM559414     1  0.7815      0.756 0.768 0.232
#&gt; GSM559422     1  0.7815      0.756 0.768 0.232
#&gt; GSM559424     1  0.7815      0.756 0.768 0.232
#&gt; GSM559431     2  0.3114      0.757 0.056 0.944
#&gt; GSM559432     2  0.7376      0.449 0.208 0.792
#&gt; GSM559381     1  0.7139      0.617 0.804 0.196
#&gt; GSM559382     2  0.7815      0.928 0.232 0.768
#&gt; GSM559384     1  0.0000      0.865 1.000 0.000
#&gt; GSM559385     1  0.0000      0.865 1.000 0.000
#&gt; GSM559386     2  0.7815      0.928 0.232 0.768
#&gt; GSM559388     2  0.7815      0.928 0.232 0.768
#&gt; GSM559389     1  0.6148      0.695 0.848 0.152
#&gt; GSM559390     1  0.0672      0.860 0.992 0.008
#&gt; GSM559392     2  0.7815      0.928 0.232 0.768
#&gt; GSM559393     2  0.8016      0.916 0.244 0.756
#&gt; GSM559394     1  0.5946      0.707 0.856 0.144
#&gt; GSM559396     1  0.1633      0.858 0.976 0.024
#&gt; GSM559398     2  0.7815      0.928 0.232 0.768
#&gt; GSM559399     1  0.6247      0.688 0.844 0.156
#&gt; GSM559400     2  0.7815      0.928 0.232 0.768
#&gt; GSM559402     1  0.0000      0.865 1.000 0.000
#&gt; GSM559403     1  0.0672      0.860 0.992 0.008
#&gt; GSM559404     1  0.3274      0.844 0.940 0.060
#&gt; GSM559405     1  0.0000      0.865 1.000 0.000
#&gt; GSM559406     1  0.0000      0.865 1.000 0.000
#&gt; GSM559407     1  0.0000      0.865 1.000 0.000
#&gt; GSM559408     1  0.0000      0.865 1.000 0.000
#&gt; GSM559409     1  0.0000      0.865 1.000 0.000
#&gt; GSM559410     1  0.0376      0.863 0.996 0.004
#&gt; GSM559411     1  0.0000      0.865 1.000 0.000
#&gt; GSM559412     1  0.0000      0.865 1.000 0.000
#&gt; GSM559413     1  0.1414      0.859 0.980 0.020
#&gt; GSM559415     1  0.8713      0.373 0.708 0.292
#&gt; GSM559416     1  0.0672      0.861 0.992 0.008
#&gt; GSM559417     2  0.9963      0.524 0.464 0.536
#&gt; GSM559418     2  0.7815      0.928 0.232 0.768
#&gt; GSM559419     1  0.0672      0.860 0.992 0.008
#&gt; GSM559420     1  0.0000      0.865 1.000 0.000
#&gt; GSM559421     2  0.7815      0.928 0.232 0.768
#&gt; GSM559423     2  0.7815      0.928 0.232 0.768
#&gt; GSM559425     2  0.7815      0.928 0.232 0.768
#&gt; GSM559426     2  0.7815      0.928 0.232 0.768
#&gt; GSM559427     2  0.7815      0.928 0.232 0.768
#&gt; GSM559428     2  0.2778      0.749 0.048 0.952
#&gt; GSM559429     2  0.7815      0.928 0.232 0.768
#&gt; GSM559430     2  0.7815      0.928 0.232 0.768
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559432     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559381     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559382     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559384     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559386     1  0.5178      0.664 0.744 0.256 0.000
#&gt; GSM559388     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559392     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559393     1  0.1031      0.961 0.976 0.024 0.000
#&gt; GSM559394     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559396     1  0.0592      0.972 0.988 0.000 0.012
#&gt; GSM559398     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559400     2  0.3340      0.834 0.120 0.880 0.000
#&gt; GSM559402     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559417     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559418     1  0.4235      0.790 0.824 0.176 0.000
#&gt; GSM559419     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559428     2  0.1163      0.962 0.000 0.972 0.028
#&gt; GSM559429     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      0.986 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.4898     0.5533 0.000 0.000 0.584 0.416
#&gt; GSM559387     3  0.3610     0.8426 0.000 0.000 0.800 0.200
#&gt; GSM559391     4  0.4916    -0.2863 0.000 0.000 0.424 0.576
#&gt; GSM559395     3  0.3528     0.8466 0.000 0.000 0.808 0.192
#&gt; GSM559397     3  0.3266     0.8532 0.000 0.000 0.832 0.168
#&gt; GSM559401     3  0.0000     0.8207 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.3074     0.8540 0.000 0.000 0.848 0.152
#&gt; GSM559422     3  0.1792     0.7935 0.000 0.000 0.932 0.068
#&gt; GSM559424     4  0.4605    -0.0161 0.000 0.000 0.336 0.664
#&gt; GSM559431     2  0.2216     0.9130 0.000 0.908 0.000 0.092
#&gt; GSM559432     3  0.1792     0.7935 0.000 0.000 0.932 0.068
#&gt; GSM559381     1  0.0817     0.8122 0.976 0.000 0.000 0.024
#&gt; GSM559382     2  0.1211     0.9229 0.000 0.960 0.000 0.040
#&gt; GSM559384     1  0.1792     0.7837 0.932 0.000 0.000 0.068
#&gt; GSM559385     1  0.0188     0.8168 0.996 0.000 0.000 0.004
#&gt; GSM559386     1  0.5856     0.0931 0.504 0.464 0.000 0.032
#&gt; GSM559388     2  0.0188     0.9411 0.000 0.996 0.000 0.004
#&gt; GSM559389     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.3764     0.6726 0.172 0.000 0.012 0.816
#&gt; GSM559392     2  0.0188     0.9411 0.000 0.996 0.000 0.004
#&gt; GSM559393     1  0.1677     0.7830 0.948 0.040 0.000 0.012
#&gt; GSM559394     1  0.0336     0.8149 0.992 0.000 0.000 0.008
#&gt; GSM559396     4  0.4004     0.5641 0.164 0.000 0.024 0.812
#&gt; GSM559398     2  0.0188     0.9411 0.000 0.996 0.000 0.004
#&gt; GSM559399     1  0.0188     0.8177 0.996 0.000 0.000 0.004
#&gt; GSM559400     2  0.5393     0.6034 0.000 0.688 0.044 0.268
#&gt; GSM559402     1  0.1557     0.7961 0.944 0.000 0.000 0.056
#&gt; GSM559403     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.4564     0.5831 0.328 0.000 0.000 0.672
#&gt; GSM559407     1  0.2149     0.7738 0.912 0.000 0.000 0.088
#&gt; GSM559408     1  0.4730     0.3549 0.636 0.000 0.000 0.364
#&gt; GSM559409     1  0.3486     0.6669 0.812 0.000 0.000 0.188
#&gt; GSM559410     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559411     4  0.4103     0.6607 0.256 0.000 0.000 0.744
#&gt; GSM559412     1  0.4804     0.2988 0.616 0.000 0.000 0.384
#&gt; GSM559413     1  0.4916     0.1617 0.576 0.000 0.000 0.424
#&gt; GSM559415     1  0.0000     0.8183 1.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.4331     0.6372 0.288 0.000 0.000 0.712
#&gt; GSM559417     4  0.6966     0.5239 0.268 0.160 0.000 0.572
#&gt; GSM559418     1  0.1940     0.7662 0.924 0.076 0.000 0.000
#&gt; GSM559419     4  0.4522     0.5953 0.320 0.000 0.000 0.680
#&gt; GSM559420     1  0.4941    -0.0347 0.564 0.000 0.000 0.436
#&gt; GSM559421     2  0.0000     0.9415 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.1389     0.9324 0.000 0.952 0.000 0.048
#&gt; GSM559425     2  0.0592     0.9398 0.000 0.984 0.000 0.016
#&gt; GSM559426     2  0.1389     0.9324 0.000 0.952 0.000 0.048
#&gt; GSM559427     2  0.0000     0.9415 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.4662     0.8272 0.000 0.796 0.092 0.112
#&gt; GSM559429     2  0.2149     0.9169 0.000 0.912 0.000 0.088
#&gt; GSM559430     2  0.0000     0.9415 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.2653      0.665 0.000 0.000 0.880 0.096 0.024
#&gt; GSM559387     3  0.0404      0.679 0.000 0.000 0.988 0.012 0.000
#&gt; GSM559391     3  0.4615      0.577 0.000 0.000 0.700 0.252 0.048
#&gt; GSM559395     3  0.0671      0.673 0.000 0.000 0.980 0.004 0.016
#&gt; GSM559397     3  0.0798      0.675 0.000 0.000 0.976 0.008 0.016
#&gt; GSM559401     3  0.4950     -0.011 0.000 0.000 0.612 0.040 0.348
#&gt; GSM559414     3  0.0794      0.662 0.000 0.000 0.972 0.000 0.028
#&gt; GSM559422     5  0.5058      0.316 0.000 0.000 0.384 0.040 0.576
#&gt; GSM559424     3  0.4866      0.548 0.000 0.000 0.664 0.284 0.052
#&gt; GSM559431     2  0.3983      0.545 0.000 0.660 0.000 0.000 0.340
#&gt; GSM559432     5  0.5058      0.317 0.000 0.000 0.384 0.040 0.576
#&gt; GSM559381     1  0.1915      0.855 0.928 0.000 0.000 0.040 0.032
#&gt; GSM559382     2  0.2477      0.708 0.008 0.892 0.000 0.092 0.008
#&gt; GSM559384     1  0.3804      0.767 0.812 0.000 0.004 0.052 0.132
#&gt; GSM559385     1  0.0324      0.867 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559386     2  0.5688      0.289 0.372 0.548 0.000 0.076 0.004
#&gt; GSM559388     2  0.1731      0.747 0.008 0.940 0.000 0.040 0.012
#&gt; GSM559389     1  0.0579      0.865 0.984 0.000 0.000 0.008 0.008
#&gt; GSM559390     4  0.1834      0.771 0.032 0.008 0.016 0.940 0.004
#&gt; GSM559392     2  0.0451      0.773 0.000 0.988 0.000 0.004 0.008
#&gt; GSM559393     1  0.1173      0.856 0.964 0.012 0.000 0.004 0.020
#&gt; GSM559394     1  0.0671      0.865 0.980 0.000 0.000 0.004 0.016
#&gt; GSM559396     3  0.7380      0.194 0.100 0.000 0.408 0.096 0.396
#&gt; GSM559398     2  0.0324      0.772 0.000 0.992 0.000 0.004 0.004
#&gt; GSM559399     1  0.0566      0.869 0.984 0.000 0.000 0.012 0.004
#&gt; GSM559400     4  0.5161      0.343 0.000 0.396 0.012 0.568 0.024
#&gt; GSM559402     1  0.3863      0.729 0.796 0.000 0.000 0.152 0.052
#&gt; GSM559403     1  0.0324      0.867 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559404     1  0.0880      0.865 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559405     1  0.0404      0.869 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559406     4  0.1928      0.796 0.072 0.004 0.000 0.920 0.004
#&gt; GSM559407     1  0.5114      0.337 0.608 0.000 0.000 0.340 0.052
#&gt; GSM559408     4  0.3662      0.702 0.252 0.000 0.000 0.744 0.004
#&gt; GSM559409     4  0.4359      0.391 0.412 0.000 0.000 0.584 0.004
#&gt; GSM559410     1  0.0963      0.863 0.964 0.000 0.000 0.036 0.000
#&gt; GSM559411     4  0.3823      0.736 0.064 0.000 0.028 0.836 0.072
#&gt; GSM559412     4  0.3696      0.746 0.212 0.000 0.000 0.772 0.016
#&gt; GSM559413     4  0.4302      0.734 0.208 0.000 0.000 0.744 0.048
#&gt; GSM559415     1  0.0671      0.869 0.980 0.000 0.000 0.016 0.004
#&gt; GSM559416     4  0.1444      0.785 0.040 0.012 0.000 0.948 0.000
#&gt; GSM559417     4  0.3209      0.763 0.032 0.088 0.000 0.864 0.016
#&gt; GSM559418     1  0.4190      0.595 0.724 0.256 0.000 0.012 0.008
#&gt; GSM559419     4  0.2052      0.798 0.080 0.004 0.000 0.912 0.004
#&gt; GSM559420     1  0.6656      0.176 0.492 0.000 0.028 0.360 0.120
#&gt; GSM559421     2  0.0566      0.776 0.000 0.984 0.000 0.004 0.012
#&gt; GSM559423     2  0.4162      0.602 0.004 0.680 0.000 0.004 0.312
#&gt; GSM559425     2  0.1851      0.758 0.000 0.912 0.000 0.000 0.088
#&gt; GSM559426     2  0.4181      0.595 0.004 0.676 0.000 0.004 0.316
#&gt; GSM559427     2  0.0510      0.775 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559428     5  0.4604     -0.424 0.000 0.428 0.012 0.000 0.560
#&gt; GSM559429     2  0.4524      0.453 0.004 0.572 0.000 0.004 0.420
#&gt; GSM559430     2  0.0880      0.775 0.000 0.968 0.000 0.000 0.032
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000     0.9230 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.1387     0.9367 0.000 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559391     3  0.0363     0.9162 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559395     3  0.1444     0.9359 0.000 0.000 0.928 0.000 0.072 0.000
#&gt; GSM559397     3  0.1556     0.9316 0.000 0.000 0.920 0.000 0.080 0.000
#&gt; GSM559401     5  0.3695     0.3809 0.000 0.000 0.376 0.000 0.624 0.000
#&gt; GSM559414     3  0.1714     0.9215 0.000 0.000 0.908 0.000 0.092 0.000
#&gt; GSM559422     5  0.1007     0.7780 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM559424     3  0.0508     0.9124 0.000 0.000 0.984 0.004 0.012 0.000
#&gt; GSM559431     6  0.5659    -0.3020 0.000 0.420 0.000 0.000 0.152 0.428
#&gt; GSM559432     5  0.1007     0.7780 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM559381     1  0.5660     0.5278 0.576 0.008 0.000 0.292 0.012 0.112
#&gt; GSM559382     2  0.2500     0.5032 0.036 0.896 0.000 0.036 0.000 0.032
#&gt; GSM559384     1  0.6234     0.4530 0.520 0.004 0.012 0.320 0.020 0.124
#&gt; GSM559385     1  0.0748     0.7759 0.976 0.016 0.000 0.004 0.004 0.000
#&gt; GSM559386     1  0.5898     0.3156 0.536 0.328 0.000 0.092 0.000 0.044
#&gt; GSM559388     2  0.4203     0.7071 0.056 0.720 0.000 0.004 0.000 0.220
#&gt; GSM559389     1  0.1546     0.7828 0.944 0.004 0.000 0.020 0.004 0.028
#&gt; GSM559390     4  0.4204     0.7212 0.004 0.272 0.036 0.688 0.000 0.000
#&gt; GSM559392     2  0.3547     0.7592 0.004 0.696 0.000 0.000 0.000 0.300
#&gt; GSM559393     1  0.1471     0.7623 0.932 0.064 0.000 0.004 0.000 0.000
#&gt; GSM559394     1  0.1074     0.7761 0.960 0.028 0.000 0.012 0.000 0.000
#&gt; GSM559396     6  0.6183     0.2397 0.008 0.000 0.208 0.220 0.020 0.544
#&gt; GSM559398     2  0.3371     0.7586 0.000 0.708 0.000 0.000 0.000 0.292
#&gt; GSM559399     1  0.2002     0.7831 0.908 0.012 0.000 0.076 0.004 0.000
#&gt; GSM559400     2  0.2848     0.3237 0.000 0.816 0.000 0.176 0.008 0.000
#&gt; GSM559402     4  0.5150     0.2951 0.280 0.004 0.008 0.640 0.016 0.052
#&gt; GSM559403     1  0.0508     0.7778 0.984 0.012 0.000 0.004 0.000 0.000
#&gt; GSM559404     1  0.2615     0.7701 0.872 0.004 0.000 0.104 0.012 0.008
#&gt; GSM559405     1  0.2113     0.7808 0.896 0.004 0.000 0.092 0.000 0.008
#&gt; GSM559406     4  0.4115     0.7228 0.004 0.268 0.032 0.696 0.000 0.000
#&gt; GSM559407     4  0.3722     0.5827 0.160 0.004 0.008 0.796 0.012 0.020
#&gt; GSM559408     4  0.3215     0.7410 0.004 0.240 0.000 0.756 0.000 0.000
#&gt; GSM559409     4  0.4853     0.6819 0.172 0.124 0.000 0.692 0.012 0.000
#&gt; GSM559410     1  0.2320     0.7665 0.864 0.004 0.000 0.132 0.000 0.000
#&gt; GSM559411     4  0.3023     0.6834 0.012 0.000 0.072 0.868 0.032 0.016
#&gt; GSM559412     4  0.1682     0.7337 0.020 0.052 0.000 0.928 0.000 0.000
#&gt; GSM559413     4  0.1967     0.7034 0.028 0.004 0.004 0.928 0.028 0.008
#&gt; GSM559415     1  0.3564     0.7031 0.768 0.004 0.000 0.204 0.000 0.024
#&gt; GSM559416     4  0.4032     0.7280 0.004 0.268 0.020 0.704 0.004 0.000
#&gt; GSM559417     4  0.3788     0.7287 0.004 0.272 0.008 0.712 0.004 0.000
#&gt; GSM559418     1  0.6737    -0.0452 0.424 0.348 0.000 0.068 0.000 0.160
#&gt; GSM559419     4  0.3483     0.7470 0.004 0.176 0.024 0.792 0.004 0.000
#&gt; GSM559420     4  0.6037     0.4411 0.096 0.008 0.044 0.640 0.020 0.192
#&gt; GSM559421     2  0.3659     0.7517 0.000 0.636 0.000 0.000 0.000 0.364
#&gt; GSM559423     6  0.1082     0.6229 0.000 0.040 0.000 0.004 0.000 0.956
#&gt; GSM559425     2  0.3747     0.7239 0.000 0.604 0.000 0.000 0.000 0.396
#&gt; GSM559426     6  0.1075     0.6129 0.000 0.048 0.000 0.000 0.000 0.952
#&gt; GSM559427     2  0.3634     0.7551 0.000 0.644 0.000 0.000 0.000 0.356
#&gt; GSM559428     6  0.3518     0.4793 0.000 0.012 0.000 0.000 0.256 0.732
#&gt; GSM559429     6  0.1168     0.6402 0.000 0.016 0.000 0.000 0.028 0.956
#&gt; GSM559430     2  0.3727     0.7356 0.000 0.612 0.000 0.000 0.000 0.388
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:NMF 50         1.22e-01 2
#> CV:NMF 52         8.27e-11 3
#> CV:NMF 45         1.58e-08 4
#> CV:NMF 41         1.10e-07 5
#> CV:NMF 42         5.89e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.685           0.833       0.931         0.3644 0.638   0.638
#> 3 3 0.385           0.617       0.801         0.5297 0.743   0.603
#> 4 4 0.408           0.609       0.741         0.1979 0.928   0.830
#> 5 5 0.577           0.615       0.756         0.1158 0.836   0.597
#> 6 6 0.642           0.567       0.755         0.0481 0.876   0.564
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559387     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559391     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559395     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559397     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559401     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559414     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559422     1  0.4298     0.8723 0.912 0.088
#&gt; GSM559424     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559431     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559432     1  0.4562     0.8644 0.904 0.096
#&gt; GSM559381     1  0.6712     0.7686 0.824 0.176
#&gt; GSM559382     1  0.9393     0.4054 0.644 0.356
#&gt; GSM559384     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559385     1  0.0672     0.9357 0.992 0.008
#&gt; GSM559386     1  0.8813     0.5456 0.700 0.300
#&gt; GSM559388     2  0.9954     0.2011 0.460 0.540
#&gt; GSM559389     1  0.3274     0.8995 0.940 0.060
#&gt; GSM559390     1  0.5294     0.8346 0.880 0.120
#&gt; GSM559392     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559393     1  0.0672     0.9357 0.992 0.008
#&gt; GSM559394     1  0.0672     0.9357 0.992 0.008
#&gt; GSM559396     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559398     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559399     1  0.0376     0.9367 0.996 0.004
#&gt; GSM559400     1  0.9996    -0.0374 0.512 0.488
#&gt; GSM559402     1  0.0376     0.9367 0.996 0.004
#&gt; GSM559403     1  0.0672     0.9357 0.992 0.008
#&gt; GSM559404     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559405     1  0.0672     0.9358 0.992 0.008
#&gt; GSM559406     1  0.0376     0.9362 0.996 0.004
#&gt; GSM559407     1  0.0376     0.9367 0.996 0.004
#&gt; GSM559408     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559409     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559410     1  0.0672     0.9357 0.992 0.008
#&gt; GSM559411     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559412     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559413     1  0.0000     0.9370 1.000 0.000
#&gt; GSM559415     1  0.2043     0.9219 0.968 0.032
#&gt; GSM559416     1  0.6048     0.8083 0.852 0.148
#&gt; GSM559417     1  0.6148     0.8032 0.848 0.152
#&gt; GSM559418     1  0.2423     0.9162 0.960 0.040
#&gt; GSM559419     1  0.0376     0.9367 0.996 0.004
#&gt; GSM559420     1  0.0376     0.9367 0.996 0.004
#&gt; GSM559421     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559423     2  0.4431     0.8109 0.092 0.908
#&gt; GSM559425     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559426     2  0.7528     0.7130 0.216 0.784
#&gt; GSM559427     2  0.0000     0.8495 0.000 1.000
#&gt; GSM559428     2  0.9996     0.1038 0.488 0.512
#&gt; GSM559429     2  0.7674     0.7036 0.224 0.776
#&gt; GSM559430     2  0.0000     0.8495 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559387     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559391     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559395     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559397     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559401     3  0.6008      0.491 0.372 0.000 0.628
#&gt; GSM559414     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559422     3  0.5845      0.464 0.308 0.004 0.688
#&gt; GSM559424     3  0.6192      0.740 0.420 0.000 0.580
#&gt; GSM559431     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559432     3  0.6172      0.461 0.308 0.012 0.680
#&gt; GSM559381     1  0.6044      0.510 0.772 0.172 0.056
#&gt; GSM559382     1  0.7150      0.260 0.616 0.348 0.036
#&gt; GSM559384     1  0.2625      0.688 0.916 0.000 0.084
#&gt; GSM559385     1  0.0424      0.725 0.992 0.000 0.008
#&gt; GSM559386     1  0.7616      0.299 0.636 0.292 0.072
#&gt; GSM559388     2  0.7446      0.271 0.432 0.532 0.036
#&gt; GSM559389     1  0.4095      0.646 0.880 0.056 0.064
#&gt; GSM559390     1  0.5695      0.572 0.804 0.120 0.076
#&gt; GSM559392     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559393     1  0.0424      0.725 0.992 0.000 0.008
#&gt; GSM559394     1  0.0424      0.725 0.992 0.000 0.008
#&gt; GSM559396     1  0.2625      0.688 0.916 0.000 0.084
#&gt; GSM559398     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559399     1  0.0592      0.725 0.988 0.000 0.012
#&gt; GSM559400     2  0.9059      0.127 0.380 0.480 0.140
#&gt; GSM559402     1  0.2165      0.714 0.936 0.000 0.064
#&gt; GSM559403     1  0.0747      0.725 0.984 0.000 0.016
#&gt; GSM559404     1  0.5678      0.411 0.684 0.000 0.316
#&gt; GSM559405     1  0.2301      0.717 0.936 0.004 0.060
#&gt; GSM559406     1  0.5785      0.469 0.696 0.004 0.300
#&gt; GSM559407     1  0.2165      0.714 0.936 0.000 0.064
#&gt; GSM559408     1  0.5016      0.533 0.760 0.000 0.240
#&gt; GSM559409     1  0.5016      0.533 0.760 0.000 0.240
#&gt; GSM559410     1  0.2261      0.717 0.932 0.000 0.068
#&gt; GSM559411     1  0.4931      0.561 0.768 0.000 0.232
#&gt; GSM559412     1  0.5678      0.411 0.684 0.000 0.316
#&gt; GSM559413     1  0.5678      0.411 0.684 0.000 0.316
#&gt; GSM559415     1  0.2056      0.707 0.952 0.024 0.024
#&gt; GSM559416     1  0.7104      0.419 0.724 0.140 0.136
#&gt; GSM559417     1  0.7163      0.412 0.720 0.144 0.136
#&gt; GSM559418     1  0.2313      0.702 0.944 0.032 0.024
#&gt; GSM559419     1  0.0000      0.726 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.726 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559423     2  0.2945      0.763 0.088 0.908 0.004
#&gt; GSM559425     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559426     2  0.4931      0.685 0.212 0.784 0.004
#&gt; GSM559427     2  0.0000      0.793 0.000 1.000 0.000
#&gt; GSM559428     2  0.7974      0.246 0.436 0.504 0.060
#&gt; GSM559429     2  0.5024      0.677 0.220 0.776 0.004
#&gt; GSM559430     2  0.0000      0.793 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.3400      0.880 0.180 0.000 0.820 0.000
#&gt; GSM559387     3  0.3400      0.880 0.180 0.000 0.820 0.000
#&gt; GSM559391     3  0.3400      0.880 0.180 0.000 0.820 0.000
#&gt; GSM559395     3  0.3400      0.880 0.180 0.000 0.820 0.000
#&gt; GSM559397     3  0.3539      0.876 0.176 0.000 0.820 0.004
#&gt; GSM559401     4  0.4985      0.833 0.000 0.000 0.468 0.532
#&gt; GSM559414     3  0.3539      0.876 0.176 0.000 0.820 0.004
#&gt; GSM559422     4  0.4697      0.921 0.000 0.000 0.356 0.644
#&gt; GSM559424     3  0.3400      0.880 0.180 0.000 0.820 0.000
#&gt; GSM559431     2  0.0000      0.778 0.000 1.000 0.000 0.000
#&gt; GSM559432     4  0.5007      0.920 0.000 0.008 0.356 0.636
#&gt; GSM559381     1  0.5652      0.577 0.736 0.080 0.012 0.172
#&gt; GSM559382     1  0.6805      0.281 0.592 0.260 0.000 0.148
#&gt; GSM559384     1  0.5175      0.608 0.760 0.000 0.120 0.120
#&gt; GSM559385     1  0.0188      0.689 0.996 0.000 0.000 0.004
#&gt; GSM559386     1  0.6756      0.360 0.612 0.188 0.000 0.200
#&gt; GSM559388     2  0.7182      0.189 0.412 0.452 0.000 0.136
#&gt; GSM559389     1  0.3896      0.651 0.844 0.012 0.024 0.120
#&gt; GSM559390     1  0.5834      0.616 0.744 0.076 0.032 0.148
#&gt; GSM559392     2  0.0000      0.778 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.0188      0.689 0.996 0.000 0.000 0.004
#&gt; GSM559394     1  0.0188      0.689 0.996 0.000 0.000 0.004
#&gt; GSM559396     1  0.5175      0.608 0.760 0.000 0.120 0.120
#&gt; GSM559398     2  0.0000      0.778 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.1151      0.687 0.968 0.000 0.024 0.008
#&gt; GSM559400     2  0.8780      0.110 0.368 0.396 0.068 0.168
#&gt; GSM559402     1  0.4956      0.624 0.776 0.000 0.108 0.116
#&gt; GSM559403     1  0.0779      0.688 0.980 0.000 0.016 0.004
#&gt; GSM559404     3  0.6833      0.261 0.272 0.000 0.584 0.144
#&gt; GSM559405     1  0.2593      0.678 0.892 0.000 0.104 0.004
#&gt; GSM559406     1  0.6665      0.343 0.544 0.000 0.360 0.096
#&gt; GSM559407     1  0.4956      0.624 0.776 0.000 0.108 0.116
#&gt; GSM559408     1  0.6887      0.305 0.528 0.000 0.356 0.116
#&gt; GSM559409     1  0.6887      0.305 0.528 0.000 0.356 0.116
#&gt; GSM559410     1  0.2675      0.679 0.892 0.000 0.100 0.008
#&gt; GSM559411     1  0.6774      0.389 0.568 0.000 0.312 0.120
#&gt; GSM559412     1  0.7047      0.105 0.440 0.000 0.440 0.120
#&gt; GSM559413     1  0.7047      0.105 0.440 0.000 0.440 0.120
#&gt; GSM559415     1  0.1902      0.679 0.932 0.000 0.004 0.064
#&gt; GSM559416     1  0.6488      0.535 0.688 0.044 0.068 0.200
#&gt; GSM559417     1  0.6523      0.532 0.684 0.044 0.068 0.204
#&gt; GSM559418     1  0.1940      0.675 0.924 0.000 0.000 0.076
#&gt; GSM559419     1  0.3278      0.662 0.864 0.000 0.020 0.116
#&gt; GSM559420     1  0.3278      0.662 0.864 0.000 0.020 0.116
#&gt; GSM559421     2  0.0336      0.776 0.000 0.992 0.000 0.008
#&gt; GSM559423     2  0.3601      0.731 0.084 0.860 0.000 0.056
#&gt; GSM559425     2  0.0000      0.778 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.5530      0.642 0.212 0.712 0.000 0.076
#&gt; GSM559427     2  0.0000      0.778 0.000 1.000 0.000 0.000
#&gt; GSM559428     1  0.7546     -0.197 0.412 0.400 0.000 0.188
#&gt; GSM559429     2  0.5598      0.635 0.220 0.704 0.000 0.076
#&gt; GSM559430     2  0.0000      0.778 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0404     0.9979 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559387     3  0.0404     0.9979 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559391     3  0.0404     0.9979 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559395     3  0.0404     0.9979 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559397     3  0.0566     0.9947 0.012 0.000 0.984 0.000 0.004
#&gt; GSM559401     5  0.4161     0.6549 0.000 0.000 0.392 0.000 0.608
#&gt; GSM559414     3  0.0566     0.9947 0.012 0.000 0.984 0.000 0.004
#&gt; GSM559422     5  0.2813     0.8604 0.000 0.000 0.168 0.000 0.832
#&gt; GSM559424     3  0.0404     0.9979 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559431     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.3093     0.8598 0.000 0.008 0.168 0.000 0.824
#&gt; GSM559381     4  0.4060     0.4837 0.360 0.000 0.000 0.640 0.000
#&gt; GSM559382     4  0.5565     0.5904 0.216 0.144 0.000 0.640 0.000
#&gt; GSM559384     1  0.4982     0.5059 0.708 0.000 0.228 0.032 0.032
#&gt; GSM559385     1  0.4417     0.5672 0.760 0.000 0.000 0.092 0.148
#&gt; GSM559386     4  0.4736     0.6144 0.216 0.072 0.000 0.712 0.000
#&gt; GSM559388     4  0.5908     0.2619 0.108 0.380 0.000 0.512 0.000
#&gt; GSM559389     1  0.4546     0.3031 0.668 0.000 0.028 0.304 0.000
#&gt; GSM559390     4  0.5061     0.3298 0.432 0.000 0.012 0.540 0.016
#&gt; GSM559392     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559393     1  0.4417     0.5672 0.760 0.000 0.000 0.092 0.148
#&gt; GSM559394     1  0.4417     0.5672 0.760 0.000 0.000 0.092 0.148
#&gt; GSM559396     1  0.4982     0.5059 0.708 0.000 0.228 0.032 0.032
#&gt; GSM559398     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.5001     0.5792 0.744 0.000 0.028 0.080 0.148
#&gt; GSM559400     4  0.8046     0.2707 0.084 0.352 0.152 0.396 0.016
#&gt; GSM559402     1  0.1597     0.6052 0.940 0.000 0.048 0.012 0.000
#&gt; GSM559403     1  0.5189     0.5764 0.732 0.000 0.032 0.088 0.148
#&gt; GSM559404     1  0.7058    -0.0308 0.376 0.000 0.360 0.252 0.012
#&gt; GSM559405     1  0.4839     0.5993 0.760 0.000 0.044 0.052 0.144
#&gt; GSM559406     1  0.6845     0.3644 0.496 0.000 0.232 0.256 0.016
#&gt; GSM559407     1  0.1597     0.6052 0.940 0.000 0.048 0.012 0.000
#&gt; GSM559408     1  0.5384     0.4849 0.664 0.000 0.228 0.104 0.004
#&gt; GSM559409     1  0.5384     0.4849 0.664 0.000 0.228 0.104 0.004
#&gt; GSM559410     1  0.4945     0.5976 0.752 0.000 0.044 0.056 0.148
#&gt; GSM559411     1  0.5007     0.5223 0.720 0.000 0.176 0.096 0.008
#&gt; GSM559412     1  0.6022     0.4279 0.592 0.000 0.268 0.132 0.008
#&gt; GSM559413     1  0.6022     0.4279 0.592 0.000 0.268 0.132 0.008
#&gt; GSM559415     1  0.5202     0.5069 0.700 0.000 0.004 0.148 0.148
#&gt; GSM559416     4  0.6606     0.3103 0.384 0.000 0.164 0.444 0.008
#&gt; GSM559417     4  0.6602     0.3178 0.380 0.000 0.164 0.448 0.008
#&gt; GSM559418     1  0.5198     0.4868 0.688 0.000 0.000 0.164 0.148
#&gt; GSM559419     1  0.1331     0.6030 0.952 0.000 0.008 0.040 0.000
#&gt; GSM559420     1  0.1331     0.6030 0.952 0.000 0.008 0.040 0.000
#&gt; GSM559421     2  0.0609     0.8542 0.000 0.980 0.000 0.020 0.000
#&gt; GSM559423     2  0.3074     0.7121 0.000 0.804 0.000 0.196 0.000
#&gt; GSM559425     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.4210     0.4423 0.000 0.588 0.000 0.412 0.000
#&gt; GSM559427     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     4  0.4983     0.3662 0.064 0.272 0.000 0.664 0.000
#&gt; GSM559429     2  0.4235     0.4204 0.000 0.576 0.000 0.424 0.000
#&gt; GSM559430     2  0.0000     0.8637 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0260     0.9981 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM559387     3  0.0260     0.9981 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM559391     3  0.0260     0.9981 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM559395     3  0.0260     0.9981 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM559397     3  0.0405     0.9951 0.000 0.000 0.988 0.008 0.004 0.000
#&gt; GSM559401     5  0.3737     0.3645 0.000 0.000 0.392 0.000 0.608 0.000
#&gt; GSM559414     3  0.0405     0.9951 0.000 0.000 0.988 0.008 0.004 0.000
#&gt; GSM559422     5  0.0000     0.7732 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559424     3  0.0260     0.9981 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM559431     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.0260     0.7719 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM559381     6  0.5102     0.3858 0.212 0.000 0.000 0.160 0.000 0.628
#&gt; GSM559382     6  0.4896     0.5527 0.212 0.120 0.000 0.004 0.000 0.664
#&gt; GSM559384     4  0.6007     0.0586 0.324 0.000 0.216 0.456 0.000 0.004
#&gt; GSM559385     1  0.3446     0.8113 0.692 0.000 0.000 0.308 0.000 0.000
#&gt; GSM559386     6  0.4031     0.5478 0.212 0.048 0.000 0.004 0.000 0.736
#&gt; GSM559388     6  0.4938     0.3286 0.076 0.356 0.000 0.000 0.000 0.568
#&gt; GSM559389     1  0.6488     0.3305 0.440 0.000 0.028 0.236 0.000 0.296
#&gt; GSM559390     6  0.5751     0.1718 0.208 0.000 0.000 0.288 0.000 0.504
#&gt; GSM559392     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559393     1  0.3446     0.8113 0.692 0.000 0.000 0.308 0.000 0.000
#&gt; GSM559394     1  0.3446     0.8113 0.692 0.000 0.000 0.308 0.000 0.000
#&gt; GSM559396     4  0.6007     0.0586 0.324 0.000 0.216 0.456 0.000 0.004
#&gt; GSM559398     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.4134     0.7910 0.656 0.000 0.028 0.316 0.000 0.000
#&gt; GSM559400     6  0.6346     0.2193 0.028 0.348 0.124 0.004 0.008 0.488
#&gt; GSM559402     4  0.3695     0.0099 0.376 0.000 0.000 0.624 0.000 0.000
#&gt; GSM559403     1  0.4118     0.7921 0.660 0.000 0.028 0.312 0.000 0.000
#&gt; GSM559404     4  0.3620     0.2104 0.248 0.000 0.008 0.736 0.000 0.008
#&gt; GSM559405     1  0.4039     0.6359 0.568 0.000 0.000 0.424 0.000 0.008
#&gt; GSM559406     4  0.4521     0.3770 0.072 0.000 0.024 0.732 0.000 0.172
#&gt; GSM559407     4  0.3695     0.0099 0.376 0.000 0.000 0.624 0.000 0.000
#&gt; GSM559408     4  0.2122     0.5171 0.076 0.000 0.024 0.900 0.000 0.000
#&gt; GSM559409     4  0.2122     0.5171 0.076 0.000 0.024 0.900 0.000 0.000
#&gt; GSM559410     1  0.3789     0.6501 0.584 0.000 0.000 0.416 0.000 0.000
#&gt; GSM559411     4  0.2135     0.4754 0.128 0.000 0.000 0.872 0.000 0.000
#&gt; GSM559412     4  0.0000     0.5104 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559413     4  0.0000     0.5104 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559415     1  0.3780     0.7721 0.728 0.000 0.004 0.248 0.000 0.020
#&gt; GSM559416     6  0.6518     0.2896 0.296 0.000 0.124 0.080 0.000 0.500
#&gt; GSM559417     6  0.6476     0.2969 0.296 0.000 0.124 0.076 0.000 0.504
#&gt; GSM559418     1  0.3791     0.7549 0.732 0.000 0.000 0.236 0.000 0.032
#&gt; GSM559419     4  0.3993    -0.3012 0.476 0.000 0.004 0.520 0.000 0.000
#&gt; GSM559420     4  0.3993    -0.3012 0.476 0.000 0.004 0.520 0.000 0.000
#&gt; GSM559421     2  0.0603     0.9383 0.004 0.980 0.000 0.000 0.000 0.016
#&gt; GSM559423     2  0.3245     0.6323 0.008 0.764 0.000 0.000 0.000 0.228
#&gt; GSM559425     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     6  0.4121     0.1108 0.016 0.380 0.000 0.000 0.000 0.604
#&gt; GSM559427     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     6  0.1327     0.4678 0.000 0.064 0.000 0.000 0.000 0.936
#&gt; GSM559429     6  0.4088     0.1366 0.016 0.368 0.000 0.000 0.000 0.616
#&gt; GSM559430     2  0.0000     0.9543 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:hclust 48         5.03e-01 2
#> MAD:hclust 38         8.40e-08 3
#> MAD:hclust 40         9.62e-08 4
#> MAD:hclust 35         3.15e-06 5
#> MAD:hclust 32         3.76e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.491           0.786       0.850         0.4141 0.538   0.538
#> 3 3 1.000           0.969       0.978         0.4717 0.740   0.563
#> 4 4 0.681           0.616       0.814         0.1623 0.876   0.696
#> 5 5 0.665           0.640       0.770         0.0882 0.910   0.720
#> 6 6 0.694           0.578       0.737         0.0555 0.851   0.504
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.0938      0.733 0.988 0.012
#&gt; GSM559387     1  0.0938      0.733 0.988 0.012
#&gt; GSM559391     1  0.0938      0.733 0.988 0.012
#&gt; GSM559395     1  0.0938      0.733 0.988 0.012
#&gt; GSM559397     1  0.0938      0.733 0.988 0.012
#&gt; GSM559401     1  0.0938      0.733 0.988 0.012
#&gt; GSM559414     1  0.0938      0.733 0.988 0.012
#&gt; GSM559422     1  0.0938      0.733 0.988 0.012
#&gt; GSM559424     1  0.0938      0.733 0.988 0.012
#&gt; GSM559431     2  0.0000      0.875 0.000 1.000
#&gt; GSM559432     2  0.8267      0.604 0.260 0.740
#&gt; GSM559381     1  0.8267      0.876 0.740 0.260
#&gt; GSM559382     2  0.0000      0.875 0.000 1.000
#&gt; GSM559384     1  0.8267      0.876 0.740 0.260
#&gt; GSM559385     1  0.8267      0.876 0.740 0.260
#&gt; GSM559386     2  0.9850     -0.105 0.428 0.572
#&gt; GSM559388     2  0.0000      0.875 0.000 1.000
#&gt; GSM559389     1  0.8267      0.876 0.740 0.260
#&gt; GSM559390     1  0.8267      0.876 0.740 0.260
#&gt; GSM559392     2  0.0000      0.875 0.000 1.000
#&gt; GSM559393     2  0.9896     -0.154 0.440 0.560
#&gt; GSM559394     1  0.8267      0.876 0.740 0.260
#&gt; GSM559396     1  0.7528      0.860 0.784 0.216
#&gt; GSM559398     2  0.0000      0.875 0.000 1.000
#&gt; GSM559399     1  0.8267      0.876 0.740 0.260
#&gt; GSM559400     2  0.0000      0.875 0.000 1.000
#&gt; GSM559402     1  0.8267      0.876 0.740 0.260
#&gt; GSM559403     1  0.8267      0.876 0.740 0.260
#&gt; GSM559404     1  0.7139      0.861 0.804 0.196
#&gt; GSM559405     1  0.8267      0.876 0.740 0.260
#&gt; GSM559406     1  0.7219      0.863 0.800 0.200
#&gt; GSM559407     1  0.8267      0.876 0.740 0.260
#&gt; GSM559408     1  0.8267      0.876 0.740 0.260
#&gt; GSM559409     1  0.8207      0.876 0.744 0.256
#&gt; GSM559410     1  0.8267      0.876 0.740 0.260
#&gt; GSM559411     1  0.7219      0.863 0.800 0.200
#&gt; GSM559412     1  0.7299      0.864 0.796 0.204
#&gt; GSM559413     1  0.7139      0.861 0.804 0.196
#&gt; GSM559415     1  0.8267      0.876 0.740 0.260
#&gt; GSM559416     1  0.8267      0.876 0.740 0.260
#&gt; GSM559417     1  0.8267      0.876 0.740 0.260
#&gt; GSM559418     2  0.9881     -0.137 0.436 0.564
#&gt; GSM559419     1  0.8267      0.876 0.740 0.260
#&gt; GSM559420     1  0.8267      0.876 0.740 0.260
#&gt; GSM559421     2  0.0000      0.875 0.000 1.000
#&gt; GSM559423     2  0.0000      0.875 0.000 1.000
#&gt; GSM559425     2  0.0000      0.875 0.000 1.000
#&gt; GSM559426     2  0.0000      0.875 0.000 1.000
#&gt; GSM559427     2  0.0000      0.875 0.000 1.000
#&gt; GSM559428     2  0.0000      0.875 0.000 1.000
#&gt; GSM559429     2  0.0000      0.875 0.000 1.000
#&gt; GSM559430     2  0.0000      0.875 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559387     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559391     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559395     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559397     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559401     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559414     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559422     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM559424     3  0.1643      0.993 0.044 0.000 0.956
#&gt; GSM559431     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559432     2  0.5785      0.559 0.000 0.668 0.332
#&gt; GSM559381     1  0.0424      0.984 0.992 0.000 0.008
#&gt; GSM559382     2  0.1643      0.948 0.000 0.956 0.044
#&gt; GSM559384     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559386     1  0.2793      0.936 0.928 0.028 0.044
#&gt; GSM559388     2  0.1529      0.949 0.000 0.960 0.040
#&gt; GSM559389     1  0.0424      0.984 0.992 0.000 0.008
#&gt; GSM559390     1  0.1529      0.962 0.960 0.000 0.040
#&gt; GSM559392     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559393     1  0.2063      0.954 0.948 0.008 0.044
#&gt; GSM559394     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559396     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559398     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559400     2  0.1643      0.948 0.000 0.956 0.044
#&gt; GSM559402     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559416     1  0.1163      0.971 0.972 0.000 0.028
#&gt; GSM559417     1  0.1529      0.962 0.960 0.000 0.040
#&gt; GSM559418     1  0.1950      0.957 0.952 0.008 0.040
#&gt; GSM559419     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559428     2  0.1643      0.948 0.000 0.956 0.044
#&gt; GSM559429     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      0.966 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.1474     0.8938 0.000 0.000 0.948 0.052
#&gt; GSM559414     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.4624     0.7026 0.000 0.000 0.660 0.340
#&gt; GSM559424     3  0.0000     0.9157 0.000 0.000 1.000 0.000
#&gt; GSM559431     2  0.0000     0.9447 0.000 1.000 0.000 0.000
#&gt; GSM559432     3  0.7694     0.3845 0.000 0.244 0.448 0.308
#&gt; GSM559381     1  0.2647     0.6881 0.880 0.000 0.000 0.120
#&gt; GSM559382     4  0.5294    -0.1328 0.008 0.484 0.000 0.508
#&gt; GSM559384     1  0.1940     0.7387 0.924 0.000 0.000 0.076
#&gt; GSM559385     1  0.2281     0.6941 0.904 0.000 0.000 0.096
#&gt; GSM559386     1  0.5755    -0.0431 0.528 0.028 0.000 0.444
#&gt; GSM559388     2  0.4790     0.2828 0.000 0.620 0.000 0.380
#&gt; GSM559389     1  0.3219     0.6554 0.836 0.000 0.000 0.164
#&gt; GSM559390     4  0.4888    -0.0429 0.412 0.000 0.000 0.588
#&gt; GSM559392     2  0.0188     0.9443 0.000 0.996 0.000 0.004
#&gt; GSM559393     4  0.4941     0.0383 0.436 0.000 0.000 0.564
#&gt; GSM559394     1  0.3024     0.6785 0.852 0.000 0.000 0.148
#&gt; GSM559396     1  0.5510     0.4188 0.600 0.000 0.024 0.376
#&gt; GSM559398     2  0.0000     0.9447 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.3975     0.5264 0.760 0.000 0.000 0.240
#&gt; GSM559400     4  0.4746     0.0170 0.000 0.368 0.000 0.632
#&gt; GSM559402     1  0.1557     0.7357 0.944 0.000 0.000 0.056
#&gt; GSM559403     1  0.2011     0.7045 0.920 0.000 0.000 0.080
#&gt; GSM559404     1  0.1940     0.7103 0.924 0.000 0.000 0.076
#&gt; GSM559405     1  0.0336     0.7303 0.992 0.000 0.000 0.008
#&gt; GSM559406     1  0.3649     0.6747 0.796 0.000 0.000 0.204
#&gt; GSM559407     1  0.1637     0.7350 0.940 0.000 0.000 0.060
#&gt; GSM559408     1  0.3569     0.6818 0.804 0.000 0.000 0.196
#&gt; GSM559409     1  0.3569     0.6818 0.804 0.000 0.000 0.196
#&gt; GSM559410     1  0.0707     0.7309 0.980 0.000 0.000 0.020
#&gt; GSM559411     1  0.3569     0.6818 0.804 0.000 0.000 0.196
#&gt; GSM559412     1  0.3569     0.6818 0.804 0.000 0.000 0.196
#&gt; GSM559413     1  0.3569     0.6818 0.804 0.000 0.000 0.196
#&gt; GSM559415     1  0.3837     0.5547 0.776 0.000 0.000 0.224
#&gt; GSM559416     4  0.4994    -0.1831 0.480 0.000 0.000 0.520
#&gt; GSM559417     4  0.4977    -0.1271 0.460 0.000 0.000 0.540
#&gt; GSM559418     1  0.4866     0.1421 0.596 0.000 0.000 0.404
#&gt; GSM559419     1  0.4961     0.2785 0.552 0.000 0.000 0.448
#&gt; GSM559420     1  0.2408     0.7333 0.896 0.000 0.000 0.104
#&gt; GSM559421     2  0.0188     0.9443 0.000 0.996 0.000 0.004
#&gt; GSM559423     2  0.0469     0.9413 0.000 0.988 0.000 0.012
#&gt; GSM559425     2  0.0000     0.9447 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0921     0.9313 0.000 0.972 0.000 0.028
#&gt; GSM559427     2  0.0000     0.9447 0.000 1.000 0.000 0.000
#&gt; GSM559428     4  0.5288    -0.1008 0.008 0.472 0.000 0.520
#&gt; GSM559429     2  0.1022     0.9281 0.000 0.968 0.000 0.032
#&gt; GSM559430     2  0.0000     0.9447 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0162     0.9678 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559387     3  0.0000     0.9671 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0162     0.9678 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559395     3  0.0162     0.9678 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559397     3  0.0000     0.9671 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.2424     0.7513 0.000 0.000 0.868 0.000 0.132
#&gt; GSM559414     3  0.0000     0.9671 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     5  0.4588     0.6890 0.000 0.000 0.380 0.016 0.604
#&gt; GSM559424     3  0.0162     0.9678 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559431     2  0.0000     0.9628 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.6318     0.7320 0.000 0.124 0.288 0.020 0.568
#&gt; GSM559381     1  0.4431     0.5385 0.732 0.000 0.000 0.216 0.052
#&gt; GSM559382     4  0.5781     0.4396 0.004 0.200 0.000 0.632 0.164
#&gt; GSM559384     1  0.1725     0.6585 0.936 0.000 0.000 0.020 0.044
#&gt; GSM559385     1  0.4933     0.5464 0.692 0.000 0.000 0.080 0.228
#&gt; GSM559386     4  0.5269     0.5294 0.180 0.024 0.000 0.712 0.084
#&gt; GSM559388     4  0.5313     0.2877 0.000 0.388 0.000 0.556 0.056
#&gt; GSM559389     1  0.5490     0.4570 0.644 0.000 0.000 0.228 0.128
#&gt; GSM559390     4  0.3039     0.5064 0.152 0.000 0.000 0.836 0.012
#&gt; GSM559392     2  0.1041     0.9507 0.000 0.964 0.000 0.004 0.032
#&gt; GSM559393     4  0.6599     0.2986 0.268 0.000 0.000 0.464 0.268
#&gt; GSM559394     1  0.5464     0.5020 0.648 0.000 0.000 0.128 0.224
#&gt; GSM559396     1  0.6602    -0.0816 0.444 0.000 0.044 0.432 0.080
#&gt; GSM559398     2  0.0955     0.9503 0.000 0.968 0.000 0.004 0.028
#&gt; GSM559399     1  0.5652     0.1379 0.552 0.000 0.000 0.360 0.088
#&gt; GSM559400     4  0.5904     0.3405 0.000 0.172 0.000 0.596 0.232
#&gt; GSM559402     1  0.0693     0.6617 0.980 0.000 0.000 0.008 0.012
#&gt; GSM559403     1  0.4212     0.5972 0.776 0.000 0.000 0.080 0.144
#&gt; GSM559404     1  0.4676     0.6121 0.720 0.000 0.000 0.072 0.208
#&gt; GSM559405     1  0.2304     0.6496 0.908 0.000 0.000 0.044 0.048
#&gt; GSM559406     1  0.5149     0.5512 0.680 0.000 0.000 0.216 0.104
#&gt; GSM559407     1  0.0807     0.6620 0.976 0.000 0.000 0.012 0.012
#&gt; GSM559408     1  0.4934     0.5718 0.708 0.000 0.000 0.188 0.104
#&gt; GSM559409     1  0.4934     0.5718 0.708 0.000 0.000 0.188 0.104
#&gt; GSM559410     1  0.2370     0.6542 0.904 0.000 0.000 0.040 0.056
#&gt; GSM559411     1  0.5006     0.5738 0.704 0.000 0.000 0.180 0.116
#&gt; GSM559412     1  0.5006     0.5715 0.704 0.000 0.000 0.180 0.116
#&gt; GSM559413     1  0.5006     0.5715 0.704 0.000 0.000 0.180 0.116
#&gt; GSM559415     1  0.5652     0.1751 0.564 0.000 0.000 0.344 0.092
#&gt; GSM559416     4  0.4479     0.4056 0.264 0.000 0.000 0.700 0.036
#&gt; GSM559417     4  0.4297     0.4457 0.236 0.000 0.000 0.728 0.036
#&gt; GSM559418     4  0.5708     0.2482 0.384 0.000 0.000 0.528 0.088
#&gt; GSM559419     4  0.4966     0.1507 0.404 0.000 0.000 0.564 0.032
#&gt; GSM559420     1  0.2676     0.6276 0.884 0.000 0.000 0.080 0.036
#&gt; GSM559421     2  0.0451     0.9615 0.000 0.988 0.000 0.008 0.004
#&gt; GSM559423     2  0.0898     0.9561 0.000 0.972 0.000 0.020 0.008
#&gt; GSM559425     2  0.0000     0.9628 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.2446     0.9045 0.000 0.900 0.000 0.056 0.044
#&gt; GSM559427     2  0.0000     0.9628 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     4  0.5604     0.4448 0.008 0.180 0.000 0.664 0.148
#&gt; GSM559429     2  0.2992     0.8769 0.000 0.868 0.000 0.068 0.064
#&gt; GSM559430     2  0.0000     0.9628 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.1074     0.9420 0.012 0.000 0.960 0.000 0.000 0.028
#&gt; GSM559387     3  0.0000     0.9413 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.1168     0.9406 0.016 0.000 0.956 0.000 0.000 0.028
#&gt; GSM559395     3  0.0820     0.9434 0.012 0.000 0.972 0.000 0.000 0.016
#&gt; GSM559397     3  0.0000     0.9413 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.2597     0.7054 0.000 0.000 0.824 0.000 0.176 0.000
#&gt; GSM559414     3  0.0000     0.9413 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.3500     0.8496 0.000 0.000 0.204 0.000 0.768 0.028
#&gt; GSM559424     3  0.1168     0.9406 0.016 0.000 0.956 0.000 0.000 0.028
#&gt; GSM559431     2  0.0000     0.9278 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.5310     0.8481 0.000 0.056 0.176 0.000 0.676 0.092
#&gt; GSM559381     1  0.6487     0.3282 0.504 0.000 0.000 0.268 0.056 0.172
#&gt; GSM559382     6  0.4387     0.6393 0.104 0.152 0.000 0.000 0.008 0.736
#&gt; GSM559384     1  0.5406     0.2337 0.556 0.000 0.000 0.356 0.048 0.040
#&gt; GSM559385     1  0.6484     0.3629 0.536 0.000 0.000 0.208 0.184 0.072
#&gt; GSM559386     6  0.3578     0.4893 0.340 0.000 0.000 0.000 0.000 0.660
#&gt; GSM559388     6  0.4859     0.5653 0.084 0.304 0.000 0.000 0.000 0.612
#&gt; GSM559389     1  0.4633     0.4738 0.736 0.000 0.000 0.148 0.036 0.080
#&gt; GSM559390     6  0.6323     0.3789 0.156 0.000 0.000 0.296 0.044 0.504
#&gt; GSM559392     2  0.1296     0.9092 0.004 0.948 0.000 0.000 0.004 0.044
#&gt; GSM559393     1  0.5625     0.1902 0.548 0.000 0.000 0.004 0.168 0.280
#&gt; GSM559394     1  0.5904     0.4228 0.620 0.000 0.000 0.144 0.168 0.068
#&gt; GSM559396     1  0.7484     0.1754 0.416 0.000 0.044 0.300 0.060 0.180
#&gt; GSM559398     2  0.1155     0.9091 0.004 0.956 0.000 0.000 0.004 0.036
#&gt; GSM559399     1  0.2579     0.4975 0.876 0.000 0.000 0.088 0.004 0.032
#&gt; GSM559400     6  0.6004     0.4453 0.088 0.080 0.000 0.000 0.248 0.584
#&gt; GSM559402     4  0.5496    -0.0263 0.404 0.000 0.000 0.508 0.048 0.040
#&gt; GSM559403     1  0.5523     0.3497 0.604 0.000 0.000 0.280 0.068 0.048
#&gt; GSM559404     4  0.5474     0.3677 0.196 0.000 0.000 0.656 0.080 0.068
#&gt; GSM559405     1  0.5089     0.1665 0.532 0.000 0.000 0.408 0.032 0.028
#&gt; GSM559406     4  0.2224     0.6695 0.020 0.000 0.000 0.904 0.012 0.064
#&gt; GSM559407     4  0.5434     0.0742 0.368 0.000 0.000 0.544 0.048 0.040
#&gt; GSM559408     4  0.0993     0.7196 0.012 0.000 0.000 0.964 0.000 0.024
#&gt; GSM559409     4  0.0806     0.7232 0.008 0.000 0.000 0.972 0.000 0.020
#&gt; GSM559410     1  0.4080     0.1365 0.536 0.000 0.000 0.456 0.008 0.000
#&gt; GSM559411     4  0.1180     0.7140 0.012 0.000 0.000 0.960 0.012 0.016
#&gt; GSM559412     4  0.0146     0.7255 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559413     4  0.0146     0.7255 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559415     1  0.3101     0.4991 0.852 0.000 0.000 0.092 0.024 0.032
#&gt; GSM559416     1  0.6986    -0.1507 0.328 0.000 0.000 0.324 0.056 0.292
#&gt; GSM559417     1  0.6950    -0.1737 0.328 0.000 0.000 0.320 0.052 0.300
#&gt; GSM559418     1  0.2793     0.4593 0.872 0.000 0.000 0.024 0.024 0.080
#&gt; GSM559419     1  0.6144     0.2030 0.492 0.000 0.000 0.336 0.032 0.140
#&gt; GSM559420     1  0.5475     0.2497 0.564 0.000 0.000 0.340 0.052 0.044
#&gt; GSM559421     2  0.0508     0.9257 0.004 0.984 0.000 0.000 0.000 0.012
#&gt; GSM559423     2  0.1036     0.9191 0.008 0.964 0.000 0.000 0.004 0.024
#&gt; GSM559425     2  0.0000     0.9278 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.3280     0.7892 0.004 0.808 0.000 0.000 0.028 0.160
#&gt; GSM559427     2  0.0000     0.9278 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     6  0.4560     0.5951 0.080 0.140 0.000 0.000 0.036 0.744
#&gt; GSM559429     2  0.4133     0.6617 0.008 0.708 0.000 0.000 0.032 0.252
#&gt; GSM559430     2  0.0000     0.9278 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:kmeans 49         5.19e-01 2
#> MAD:kmeans 52         9.23e-10 3
#> MAD:kmeans 39         3.60e-08 4
#> MAD:kmeans 39         5.73e-07 5
#> MAD:kmeans 29         4.63e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.847           0.945       0.976         0.4862 0.517   0.517
#> 3 3 1.000           0.960       0.985         0.3389 0.773   0.586
#> 4 4 0.844           0.852       0.908         0.1514 0.851   0.600
#> 5 5 0.801           0.688       0.830         0.0582 0.857   0.526
#> 6 6 0.772           0.549       0.783         0.0341 0.990   0.955
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.000      0.972 1.000 0.000
#&gt; GSM559387     1   0.000      0.972 1.000 0.000
#&gt; GSM559391     1   0.000      0.972 1.000 0.000
#&gt; GSM559395     1   0.000      0.972 1.000 0.000
#&gt; GSM559397     1   0.000      0.972 1.000 0.000
#&gt; GSM559401     1   0.000      0.972 1.000 0.000
#&gt; GSM559414     1   0.000      0.972 1.000 0.000
#&gt; GSM559422     2   0.839      0.625 0.268 0.732
#&gt; GSM559424     1   0.000      0.972 1.000 0.000
#&gt; GSM559431     2   0.000      0.975 0.000 1.000
#&gt; GSM559432     2   0.000      0.975 0.000 1.000
#&gt; GSM559381     1   0.722      0.752 0.800 0.200
#&gt; GSM559382     2   0.000      0.975 0.000 1.000
#&gt; GSM559384     1   0.000      0.972 1.000 0.000
#&gt; GSM559385     1   0.000      0.972 1.000 0.000
#&gt; GSM559386     2   0.000      0.975 0.000 1.000
#&gt; GSM559388     2   0.000      0.975 0.000 1.000
#&gt; GSM559389     1   0.722      0.752 0.800 0.200
#&gt; GSM559390     1   0.605      0.821 0.852 0.148
#&gt; GSM559392     2   0.000      0.975 0.000 1.000
#&gt; GSM559393     2   0.000      0.975 0.000 1.000
#&gt; GSM559394     1   0.000      0.972 1.000 0.000
#&gt; GSM559396     1   0.000      0.972 1.000 0.000
#&gt; GSM559398     2   0.000      0.975 0.000 1.000
#&gt; GSM559399     1   0.000      0.972 1.000 0.000
#&gt; GSM559400     2   0.000      0.975 0.000 1.000
#&gt; GSM559402     1   0.000      0.972 1.000 0.000
#&gt; GSM559403     1   0.000      0.972 1.000 0.000
#&gt; GSM559404     1   0.000      0.972 1.000 0.000
#&gt; GSM559405     1   0.000      0.972 1.000 0.000
#&gt; GSM559406     1   0.000      0.972 1.000 0.000
#&gt; GSM559407     1   0.000      0.972 1.000 0.000
#&gt; GSM559408     1   0.000      0.972 1.000 0.000
#&gt; GSM559409     1   0.000      0.972 1.000 0.000
#&gt; GSM559410     1   0.000      0.972 1.000 0.000
#&gt; GSM559411     1   0.000      0.972 1.000 0.000
#&gt; GSM559412     1   0.000      0.972 1.000 0.000
#&gt; GSM559413     1   0.000      0.972 1.000 0.000
#&gt; GSM559415     1   0.000      0.972 1.000 0.000
#&gt; GSM559416     1   0.833      0.643 0.736 0.264
#&gt; GSM559417     2   0.671      0.775 0.176 0.824
#&gt; GSM559418     2   0.000      0.975 0.000 1.000
#&gt; GSM559419     1   0.000      0.972 1.000 0.000
#&gt; GSM559420     1   0.000      0.972 1.000 0.000
#&gt; GSM559421     2   0.000      0.975 0.000 1.000
#&gt; GSM559423     2   0.000      0.975 0.000 1.000
#&gt; GSM559425     2   0.000      0.975 0.000 1.000
#&gt; GSM559426     2   0.000      0.975 0.000 1.000
#&gt; GSM559427     2   0.000      0.975 0.000 1.000
#&gt; GSM559428     2   0.000      0.975 0.000 1.000
#&gt; GSM559429     2   0.000      0.975 0.000 1.000
#&gt; GSM559430     2   0.000      0.975 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559432     3  0.3267     0.8680 0.000 0.116 0.884
#&gt; GSM559381     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559382     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559384     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559386     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559388     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559390     1  0.3340     0.8460 0.880 0.000 0.120
#&gt; GSM559392     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559393     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559394     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559396     3  0.0000     0.9882 0.000 0.000 1.000
#&gt; GSM559398     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559400     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559402     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559406     1  0.1163     0.9461 0.972 0.000 0.028
#&gt; GSM559407     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559417     1  0.6308     0.0302 0.508 0.492 0.000
#&gt; GSM559418     2  0.0747     0.9806 0.016 0.984 0.000
#&gt; GSM559419     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000     0.9698 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559428     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559429     2  0.0000     0.9988 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000     0.9988 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559424     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559431     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559432     3  0.4898      0.286 0.000 0.416 0.584 0.000
#&gt; GSM559381     1  0.4431      0.816 0.696 0.000 0.000 0.304
#&gt; GSM559382     2  0.0188      0.989 0.004 0.996 0.000 0.000
#&gt; GSM559384     1  0.4382      0.817 0.704 0.000 0.000 0.296
#&gt; GSM559385     1  0.4040      0.819 0.752 0.000 0.000 0.248
#&gt; GSM559386     2  0.2737      0.886 0.104 0.888 0.000 0.008
#&gt; GSM559388     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559389     1  0.4008      0.819 0.756 0.000 0.000 0.244
#&gt; GSM559390     4  0.3486      0.777 0.188 0.000 0.000 0.812
#&gt; GSM559392     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.4804      0.355 0.616 0.384 0.000 0.000
#&gt; GSM559394     1  0.3801      0.814 0.780 0.000 0.000 0.220
#&gt; GSM559396     3  0.0000      0.951 0.000 0.000 1.000 0.000
#&gt; GSM559398     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.0469      0.664 0.988 0.000 0.000 0.012
#&gt; GSM559400     2  0.0469      0.982 0.000 0.988 0.000 0.012
#&gt; GSM559402     1  0.4804      0.753 0.616 0.000 0.000 0.384
#&gt; GSM559403     1  0.4072      0.819 0.748 0.000 0.000 0.252
#&gt; GSM559404     1  0.4431      0.817 0.696 0.000 0.000 0.304
#&gt; GSM559405     1  0.4356      0.819 0.708 0.000 0.000 0.292
#&gt; GSM559406     4  0.1610      0.813 0.016 0.000 0.032 0.952
#&gt; GSM559407     1  0.4877      0.722 0.592 0.000 0.000 0.408
#&gt; GSM559408     4  0.1022      0.819 0.032 0.000 0.000 0.968
#&gt; GSM559409     4  0.1118      0.818 0.036 0.000 0.000 0.964
#&gt; GSM559410     1  0.4543      0.808 0.676 0.000 0.000 0.324
#&gt; GSM559411     4  0.1118      0.818 0.036 0.000 0.000 0.964
#&gt; GSM559412     4  0.1118      0.818 0.036 0.000 0.000 0.964
#&gt; GSM559413     4  0.1118      0.818 0.036 0.000 0.000 0.964
#&gt; GSM559415     1  0.1022      0.647 0.968 0.000 0.000 0.032
#&gt; GSM559416     4  0.4220      0.750 0.248 0.004 0.000 0.748
#&gt; GSM559417     4  0.4220      0.750 0.248 0.004 0.000 0.748
#&gt; GSM559418     1  0.2596      0.596 0.908 0.068 0.000 0.024
#&gt; GSM559419     4  0.4040      0.752 0.248 0.000 0.000 0.752
#&gt; GSM559420     1  0.4277      0.724 0.720 0.000 0.000 0.280
#&gt; GSM559421     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.0188      0.989 0.004 0.996 0.000 0.000
#&gt; GSM559429     2  0.0000      0.991 0.000 1.000 0.000 0.000
#&gt; GSM559430     2  0.0000      0.991 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.2983    0.86648 0.000 0.000 0.864 0.040 0.096
#&gt; GSM559424     3  0.0000    0.98311 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     2  0.5318    0.05860 0.000 0.496 0.460 0.040 0.004
#&gt; GSM559381     1  0.6545    0.40954 0.464 0.000 0.000 0.220 0.316
#&gt; GSM559382     2  0.1281    0.90450 0.000 0.956 0.000 0.012 0.032
#&gt; GSM559384     1  0.6528    0.44998 0.480 0.000 0.000 0.236 0.284
#&gt; GSM559385     5  0.0955    0.86656 0.028 0.000 0.000 0.004 0.968
#&gt; GSM559386     2  0.5648    0.60395 0.020 0.680 0.000 0.160 0.140
#&gt; GSM559388     2  0.0579    0.92136 0.000 0.984 0.000 0.008 0.008
#&gt; GSM559389     5  0.3037    0.80959 0.040 0.000 0.000 0.100 0.860
#&gt; GSM559390     1  0.4886   -0.00479 0.596 0.000 0.000 0.372 0.032
#&gt; GSM559392     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559393     5  0.2588    0.76212 0.000 0.048 0.000 0.060 0.892
#&gt; GSM559394     5  0.1106    0.86151 0.024 0.000 0.000 0.012 0.964
#&gt; GSM559396     3  0.0912    0.96124 0.012 0.000 0.972 0.016 0.000
#&gt; GSM559398     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     4  0.4708    0.19633 0.016 0.000 0.000 0.548 0.436
#&gt; GSM559400     2  0.2674    0.81854 0.000 0.856 0.000 0.140 0.004
#&gt; GSM559402     1  0.5984    0.54385 0.588 0.000 0.000 0.208 0.204
#&gt; GSM559403     5  0.2329    0.79003 0.124 0.000 0.000 0.000 0.876
#&gt; GSM559404     1  0.5512    0.53573 0.620 0.000 0.000 0.104 0.276
#&gt; GSM559405     1  0.6218    0.41019 0.488 0.000 0.000 0.148 0.364
#&gt; GSM559406     1  0.3002    0.47664 0.856 0.000 0.028 0.116 0.000
#&gt; GSM559407     1  0.5904    0.54937 0.600 0.000 0.000 0.196 0.204
#&gt; GSM559408     1  0.0880    0.57464 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559409     1  0.0703    0.57883 0.976 0.000 0.000 0.024 0.000
#&gt; GSM559410     1  0.5968    0.36864 0.512 0.000 0.000 0.116 0.372
#&gt; GSM559411     1  0.0880    0.59265 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559412     1  0.0290    0.58820 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559413     1  0.0000    0.58982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559415     4  0.4294    0.21349 0.000 0.000 0.000 0.532 0.468
#&gt; GSM559416     4  0.3837    0.45739 0.308 0.000 0.000 0.692 0.000
#&gt; GSM559417     4  0.4047    0.44280 0.320 0.000 0.000 0.676 0.004
#&gt; GSM559418     4  0.4590    0.26485 0.000 0.012 0.000 0.568 0.420
#&gt; GSM559419     4  0.3990    0.46208 0.308 0.000 0.000 0.688 0.004
#&gt; GSM559420     4  0.6169   -0.21410 0.392 0.000 0.000 0.472 0.136
#&gt; GSM559421     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559425     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.1012    0.91337 0.000 0.968 0.000 0.020 0.012
#&gt; GSM559429     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559430     2  0.0000    0.92782 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559395     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0858    0.91986 0.004 0.000 0.968 0.000 0.000 0.028
#&gt; GSM559414     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     3  0.5698    0.45415 0.176 0.000 0.556 0.000 0.008 0.260
#&gt; GSM559424     3  0.0000    0.93756 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559431     2  0.0000    0.85963 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     2  0.6579    0.07712 0.012 0.388 0.312 0.000 0.008 0.280
#&gt; GSM559381     6  0.7076    0.00000 0.196 0.000 0.000 0.308 0.092 0.404
#&gt; GSM559382     2  0.3499    0.76579 0.032 0.796 0.000 0.000 0.008 0.164
#&gt; GSM559384     4  0.7481   -0.41494 0.168 0.000 0.000 0.376 0.212 0.244
#&gt; GSM559385     1  0.1708    0.78967 0.932 0.000 0.000 0.040 0.004 0.024
#&gt; GSM559386     2  0.7019    0.24612 0.100 0.428 0.000 0.016 0.100 0.356
#&gt; GSM559388     2  0.1781    0.83569 0.008 0.924 0.000 0.000 0.008 0.060
#&gt; GSM559389     1  0.5169    0.49319 0.672 0.000 0.000 0.056 0.060 0.212
#&gt; GSM559390     4  0.6526   -0.17084 0.020 0.000 0.000 0.360 0.340 0.280
#&gt; GSM559392     2  0.0260    0.85841 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559393     1  0.2317    0.72101 0.900 0.016 0.000 0.000 0.020 0.064
#&gt; GSM559394     1  0.1218    0.78556 0.956 0.000 0.000 0.028 0.012 0.004
#&gt; GSM559396     3  0.2661    0.83310 0.000 0.000 0.872 0.016 0.016 0.096
#&gt; GSM559398     2  0.0260    0.85841 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559399     5  0.5232    0.38517 0.316 0.000 0.000 0.012 0.588 0.084
#&gt; GSM559400     2  0.5282    0.49113 0.004 0.584 0.000 0.000 0.116 0.296
#&gt; GSM559402     4  0.6445   -0.07341 0.108 0.000 0.000 0.564 0.148 0.180
#&gt; GSM559403     1  0.3351    0.66181 0.820 0.000 0.000 0.136 0.016 0.028
#&gt; GSM559404     4  0.4484    0.24049 0.160 0.000 0.000 0.744 0.056 0.040
#&gt; GSM559405     4  0.6716   -0.23296 0.252 0.000 0.000 0.504 0.104 0.140
#&gt; GSM559406     4  0.3627    0.39693 0.004 0.000 0.052 0.824 0.096 0.024
#&gt; GSM559407     4  0.6043    0.03652 0.100 0.000 0.000 0.616 0.136 0.148
#&gt; GSM559408     4  0.1082    0.49525 0.000 0.000 0.000 0.956 0.040 0.004
#&gt; GSM559409     4  0.0692    0.50274 0.000 0.000 0.000 0.976 0.020 0.004
#&gt; GSM559410     4  0.6094   -0.00339 0.268 0.000 0.000 0.556 0.124 0.052
#&gt; GSM559411     4  0.1245    0.48480 0.000 0.000 0.000 0.952 0.016 0.032
#&gt; GSM559412     4  0.0547    0.50336 0.000 0.000 0.000 0.980 0.020 0.000
#&gt; GSM559413     4  0.0260    0.50200 0.000 0.000 0.000 0.992 0.008 0.000
#&gt; GSM559415     5  0.4486    0.36872 0.384 0.000 0.000 0.004 0.584 0.028
#&gt; GSM559416     5  0.3551    0.50541 0.000 0.000 0.000 0.168 0.784 0.048
#&gt; GSM559417     5  0.4314    0.47037 0.000 0.000 0.000 0.184 0.720 0.096
#&gt; GSM559418     5  0.4774    0.40543 0.336 0.020 0.000 0.000 0.612 0.032
#&gt; GSM559419     5  0.3377    0.50783 0.000 0.000 0.000 0.188 0.784 0.028
#&gt; GSM559420     5  0.7102   -0.30176 0.100 0.000 0.000 0.236 0.432 0.232
#&gt; GSM559421     2  0.0000    0.85963 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0363    0.85805 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM559425     2  0.0000    0.85963 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.1411    0.84402 0.000 0.936 0.000 0.000 0.004 0.060
#&gt; GSM559427     2  0.0000    0.85963 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.3089    0.77139 0.004 0.800 0.000 0.000 0.008 0.188
#&gt; GSM559429     2  0.1908    0.83012 0.000 0.900 0.000 0.000 0.004 0.096
#&gt; GSM559430     2  0.0000    0.85963 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> MAD:skmeans 52         6.10e-01 2
#> MAD:skmeans 51         2.00e-09 3
#> MAD:skmeans 50         2.17e-08 4
#> MAD:skmeans 38         2.79e-06 5
#> MAD:skmeans 31         1.82e-04 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.473           0.747       0.840         0.3720 0.599   0.599
#> 3 3 0.967           0.927       0.973         0.6302 0.633   0.463
#> 4 4 0.816           0.878       0.934         0.2014 0.843   0.625
#> 5 5 0.829           0.881       0.931         0.0240 0.986   0.948
#> 6 6 0.719           0.629       0.830         0.0523 0.946   0.803
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2   0.000     0.8100 0.000 1.000
#&gt; GSM559387     2   0.000     0.8100 0.000 1.000
#&gt; GSM559391     2   0.000     0.8100 0.000 1.000
#&gt; GSM559395     2   0.000     0.8100 0.000 1.000
#&gt; GSM559397     2   0.000     0.8100 0.000 1.000
#&gt; GSM559401     2   0.000     0.8100 0.000 1.000
#&gt; GSM559414     2   0.000     0.8100 0.000 1.000
#&gt; GSM559422     2   0.000     0.8100 0.000 1.000
#&gt; GSM559424     2   0.000     0.8100 0.000 1.000
#&gt; GSM559431     1   0.000     0.7287 1.000 0.000
#&gt; GSM559432     2   0.821     0.5685 0.256 0.744
#&gt; GSM559381     1   0.821     0.8521 0.744 0.256
#&gt; GSM559382     1   0.821     0.8521 0.744 0.256
#&gt; GSM559384     1   0.821     0.8521 0.744 0.256
#&gt; GSM559385     1   0.821     0.8521 0.744 0.256
#&gt; GSM559386     1   0.821     0.8521 0.744 0.256
#&gt; GSM559388     1   0.000     0.7287 1.000 0.000
#&gt; GSM559389     1   0.821     0.8521 0.744 0.256
#&gt; GSM559390     1   0.821     0.8521 0.744 0.256
#&gt; GSM559392     1   0.000     0.7287 1.000 0.000
#&gt; GSM559393     1   0.821     0.8521 0.744 0.256
#&gt; GSM559394     1   0.821     0.8521 0.744 0.256
#&gt; GSM559396     2   0.985    -0.0779 0.428 0.572
#&gt; GSM559398     1   0.000     0.7287 1.000 0.000
#&gt; GSM559399     1   0.821     0.8521 0.744 0.256
#&gt; GSM559400     1   0.295     0.6696 0.948 0.052
#&gt; GSM559402     1   0.821     0.8521 0.744 0.256
#&gt; GSM559403     1   0.821     0.8521 0.744 0.256
#&gt; GSM559404     2   0.997    -0.2184 0.468 0.532
#&gt; GSM559405     1   0.821     0.8521 0.744 0.256
#&gt; GSM559406     2   0.861     0.4397 0.284 0.716
#&gt; GSM559407     1   0.821     0.8521 0.744 0.256
#&gt; GSM559408     1   0.821     0.8521 0.744 0.256
#&gt; GSM559409     1   0.821     0.8521 0.744 0.256
#&gt; GSM559410     1   0.821     0.8521 0.744 0.256
#&gt; GSM559411     1   0.881     0.7922 0.700 0.300
#&gt; GSM559412     1   0.821     0.8521 0.744 0.256
#&gt; GSM559413     2   0.958     0.1588 0.380 0.620
#&gt; GSM559415     1   0.821     0.8521 0.744 0.256
#&gt; GSM559416     1   0.821     0.8521 0.744 0.256
#&gt; GSM559417     1   0.821     0.8521 0.744 0.256
#&gt; GSM559418     1   0.821     0.8521 0.744 0.256
#&gt; GSM559419     1   0.821     0.8521 0.744 0.256
#&gt; GSM559420     1   0.821     0.8521 0.744 0.256
#&gt; GSM559421     1   0.000     0.7287 1.000 0.000
#&gt; GSM559423     1   0.000     0.7287 1.000 0.000
#&gt; GSM559425     1   0.000     0.7287 1.000 0.000
#&gt; GSM559426     1   0.000     0.7287 1.000 0.000
#&gt; GSM559427     1   0.000     0.7287 1.000 0.000
#&gt; GSM559428     1   0.163     0.7399 0.976 0.024
#&gt; GSM559429     1   0.000     0.7287 1.000 0.000
#&gt; GSM559430     1   0.000     0.7287 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559432     2  0.6295      0.120 0.000 0.528 0.472
#&gt; GSM559381     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559382     1  0.5760      0.506 0.672 0.328 0.000
#&gt; GSM559384     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559386     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559388     2  0.0592      0.927 0.012 0.988 0.000
#&gt; GSM559389     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559392     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559393     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559394     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559396     1  0.6062      0.380 0.616 0.000 0.384
#&gt; GSM559398     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559400     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559402     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559406     1  0.0237      0.970 0.996 0.000 0.004
#&gt; GSM559407     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559415     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559417     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559418     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559419     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559428     2  0.4750      0.668 0.216 0.784 0.000
#&gt; GSM559429     2  0.0000      0.939 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      0.939 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.0188      0.995 0.004 0.000 0.996 0.000
#&gt; GSM559424     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM559431     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559432     2  0.4985      0.129 0.000 0.532 0.468 0.000
#&gt; GSM559381     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559382     4  0.3688      0.845 0.208 0.000 0.000 0.792
#&gt; GSM559384     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559386     4  0.3688      0.845 0.208 0.000 0.000 0.792
#&gt; GSM559388     4  0.3870      0.844 0.208 0.004 0.000 0.788
#&gt; GSM559389     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000      0.835 0.000 0.000 0.000 1.000
#&gt; GSM559392     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559394     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559396     1  0.4790      0.443 0.620 0.000 0.380 0.000
#&gt; GSM559398     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559400     4  0.4499      0.835 0.160 0.048 0.000 0.792
#&gt; GSM559402     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.0000      0.835 0.000 0.000 0.000 1.000
#&gt; GSM559407     1  0.2814      0.842 0.868 0.000 0.000 0.132
#&gt; GSM559408     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559409     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559410     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559411     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559412     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559413     1  0.3688      0.800 0.792 0.000 0.000 0.208
#&gt; GSM559415     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.0000      0.835 0.000 0.000 0.000 1.000
#&gt; GSM559417     4  0.0000      0.835 0.000 0.000 0.000 1.000
#&gt; GSM559418     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559419     4  0.2921      0.773 0.140 0.000 0.000 0.860
#&gt; GSM559420     1  0.0000      0.901 1.000 0.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559428     4  0.3688      0.845 0.208 0.000 0.000 0.792
#&gt; GSM559429     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM559430     2  0.0000      0.951 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     5  0.1908      1.000 0.000 0.000 0.092 0.000 0.908
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.1908      1.000 0.000 0.000 0.092 0.000 0.908
#&gt; GSM559381     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     4  0.4205      0.750 0.208 0.008 0.000 0.756 0.028
#&gt; GSM559384     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559386     4  0.3333      0.754 0.208 0.000 0.000 0.788 0.004
#&gt; GSM559388     4  0.4592      0.743 0.208 0.024 0.000 0.740 0.028
#&gt; GSM559389     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000      0.780 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559392     2  0.0609      0.979 0.000 0.980 0.000 0.000 0.020
#&gt; GSM559393     1  0.0162      0.895 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559394     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559396     1  0.5113      0.448 0.604 0.000 0.352 0.004 0.040
#&gt; GSM559398     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559400     4  0.3455      0.668 0.008 0.000 0.000 0.784 0.208
#&gt; GSM559402     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.0162      0.779 0.004 0.000 0.000 0.996 0.000
#&gt; GSM559407     1  0.2424      0.837 0.868 0.000 0.000 0.132 0.000
#&gt; GSM559408     1  0.3210      0.790 0.788 0.000 0.000 0.212 0.000
#&gt; GSM559409     1  0.4224      0.763 0.744 0.000 0.000 0.216 0.040
#&gt; GSM559410     1  0.3177      0.793 0.792 0.000 0.000 0.208 0.000
#&gt; GSM559411     1  0.4224      0.763 0.744 0.000 0.000 0.216 0.040
#&gt; GSM559412     1  0.3210      0.790 0.788 0.000 0.000 0.212 0.000
#&gt; GSM559413     1  0.4193      0.765 0.748 0.000 0.000 0.212 0.040
#&gt; GSM559415     1  0.0000      0.897 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.0000      0.780 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559417     4  0.0000      0.780 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559418     1  0.0162      0.895 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559419     4  0.3262      0.682 0.124 0.000 0.000 0.840 0.036
#&gt; GSM559420     1  0.0162      0.895 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559421     2  0.0609      0.979 0.000 0.980 0.000 0.000 0.020
#&gt; GSM559423     2  0.0404      0.984 0.000 0.988 0.000 0.000 0.012
#&gt; GSM559425     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0703      0.978 0.000 0.976 0.000 0.000 0.024
#&gt; GSM559427     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     4  0.4369      0.744 0.208 0.000 0.000 0.740 0.052
#&gt; GSM559429     2  0.0880      0.974 0.000 0.968 0.000 0.000 0.032
#&gt; GSM559430     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559395     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559414     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559424     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559431     2  0.0000     0.8931 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559381     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     6  0.5655     0.9203 0.132 0.004 0.000 0.408 0.000 0.456
#&gt; GSM559384     1  0.2092     0.7708 0.876 0.000 0.000 0.124 0.000 0.000
#&gt; GSM559385     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559386     4  0.4558    -0.3085 0.132 0.000 0.000 0.700 0.000 0.168
#&gt; GSM559388     6  0.5767     0.9202 0.124 0.012 0.000 0.392 0.000 0.472
#&gt; GSM559389     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.2340     0.1941 0.000 0.000 0.000 0.852 0.000 0.148
#&gt; GSM559392     2  0.2996     0.7434 0.000 0.772 0.000 0.000 0.000 0.228
#&gt; GSM559393     1  0.3954     0.3439 0.636 0.000 0.000 0.012 0.000 0.352
#&gt; GSM559394     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559396     4  0.6319     0.0728 0.308 0.000 0.036 0.488 0.000 0.168
#&gt; GSM559398     2  0.2597     0.7815 0.000 0.824 0.000 0.000 0.000 0.176
#&gt; GSM559399     1  0.4088     0.2623 0.616 0.000 0.000 0.368 0.000 0.016
#&gt; GSM559400     4  0.5466    -0.6624 0.000 0.000 0.000 0.472 0.124 0.404
#&gt; GSM559402     1  0.2092     0.7708 0.876 0.000 0.000 0.124 0.000 0.000
#&gt; GSM559403     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559404     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.0000     0.7956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.5571    -0.0130 0.356 0.000 0.000 0.496 0.000 0.148
#&gt; GSM559407     1  0.2053     0.7769 0.888 0.000 0.000 0.108 0.000 0.004
#&gt; GSM559408     1  0.2706     0.7583 0.852 0.000 0.000 0.024 0.000 0.124
#&gt; GSM559409     1  0.4037     0.6897 0.736 0.000 0.000 0.064 0.000 0.200
#&gt; GSM559410     1  0.2346     0.7657 0.868 0.000 0.000 0.008 0.000 0.124
#&gt; GSM559411     1  0.4494     0.6398 0.692 0.000 0.000 0.092 0.000 0.216
#&gt; GSM559412     1  0.2790     0.7557 0.844 0.000 0.000 0.024 0.000 0.132
#&gt; GSM559413     1  0.3876     0.6382 0.700 0.000 0.000 0.024 0.000 0.276
#&gt; GSM559415     1  0.3778     0.5556 0.696 0.000 0.000 0.288 0.000 0.016
#&gt; GSM559416     4  0.0458     0.2138 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM559417     4  0.2219     0.2054 0.000 0.000 0.000 0.864 0.000 0.136
#&gt; GSM559418     1  0.4121     0.2362 0.604 0.000 0.000 0.380 0.000 0.016
#&gt; GSM559419     4  0.3717     0.2784 0.072 0.000 0.000 0.780 0.000 0.148
#&gt; GSM559420     4  0.3868    -0.2749 0.496 0.000 0.000 0.504 0.000 0.000
#&gt; GSM559421     2  0.1141     0.8725 0.000 0.948 0.000 0.000 0.000 0.052
#&gt; GSM559423     2  0.0363     0.8897 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM559425     2  0.0000     0.8931 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0790     0.8819 0.000 0.968 0.000 0.000 0.000 0.032
#&gt; GSM559427     2  0.0000     0.8931 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     4  0.5390    -0.5519 0.132 0.000 0.000 0.540 0.000 0.328
#&gt; GSM559429     2  0.3797     0.4527 0.000 0.580 0.000 0.000 0.000 0.420
#&gt; GSM559430     2  0.0000     0.8931 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:pam 48         1.09e-09 2
#> MAD:pam 50         2.49e-10 3
#> MAD:pam 50         1.26e-09 4
#> MAD:pam 51         2.87e-09 5
#> MAD:pam 38         8.67e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.978       0.974         0.3244 0.683   0.683
#> 3 3 0.979           0.962       0.977         0.9025 0.704   0.567
#> 4 4 0.770           0.819       0.871         0.1028 0.988   0.969
#> 5 5 0.736           0.771       0.802         0.0905 0.834   0.572
#> 6 6 0.858           0.864       0.921         0.0715 0.952   0.800
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.2778      1.000 0.048 0.952
#&gt; GSM559387     2  0.2778      1.000 0.048 0.952
#&gt; GSM559391     2  0.2778      1.000 0.048 0.952
#&gt; GSM559395     2  0.2778      1.000 0.048 0.952
#&gt; GSM559397     2  0.2778      1.000 0.048 0.952
#&gt; GSM559401     2  0.2778      1.000 0.048 0.952
#&gt; GSM559414     2  0.2778      1.000 0.048 0.952
#&gt; GSM559422     2  0.2778      1.000 0.048 0.952
#&gt; GSM559424     2  0.2778      1.000 0.048 0.952
#&gt; GSM559431     1  0.3114      0.961 0.944 0.056
#&gt; GSM559432     2  0.2778      1.000 0.048 0.952
#&gt; GSM559381     1  0.0376      0.979 0.996 0.004
#&gt; GSM559382     1  0.2778      0.964 0.952 0.048
#&gt; GSM559384     1  0.0000      0.979 1.000 0.000
#&gt; GSM559385     1  0.0000      0.979 1.000 0.000
#&gt; GSM559386     1  0.0376      0.979 0.996 0.004
#&gt; GSM559388     1  0.3114      0.961 0.944 0.056
#&gt; GSM559389     1  0.0000      0.979 1.000 0.000
#&gt; GSM559390     1  0.0938      0.975 0.988 0.012
#&gt; GSM559392     1  0.3114      0.961 0.944 0.056
#&gt; GSM559393     1  0.0000      0.979 1.000 0.000
#&gt; GSM559394     1  0.0000      0.979 1.000 0.000
#&gt; GSM559396     1  0.1414      0.973 0.980 0.020
#&gt; GSM559398     1  0.3114      0.961 0.944 0.056
#&gt; GSM559399     1  0.0376      0.979 0.996 0.004
#&gt; GSM559400     1  0.2236      0.972 0.964 0.036
#&gt; GSM559402     1  0.0376      0.979 0.996 0.004
#&gt; GSM559403     1  0.0000      0.979 1.000 0.000
#&gt; GSM559404     1  0.0000      0.979 1.000 0.000
#&gt; GSM559405     1  0.0000      0.979 1.000 0.000
#&gt; GSM559406     1  0.0938      0.975 0.988 0.012
#&gt; GSM559407     1  0.0376      0.979 0.996 0.004
#&gt; GSM559408     1  0.0938      0.975 0.988 0.012
#&gt; GSM559409     1  0.0000      0.979 1.000 0.000
#&gt; GSM559410     1  0.0000      0.979 1.000 0.000
#&gt; GSM559411     1  0.0938      0.975 0.988 0.012
#&gt; GSM559412     1  0.0938      0.975 0.988 0.012
#&gt; GSM559413     1  0.0938      0.975 0.988 0.012
#&gt; GSM559415     1  0.0376      0.979 0.996 0.004
#&gt; GSM559416     1  0.0938      0.975 0.988 0.012
#&gt; GSM559417     1  0.0938      0.975 0.988 0.012
#&gt; GSM559418     1  0.0000      0.979 1.000 0.000
#&gt; GSM559419     1  0.0672      0.976 0.992 0.008
#&gt; GSM559420     1  0.0000      0.979 1.000 0.000
#&gt; GSM559421     1  0.3114      0.961 0.944 0.056
#&gt; GSM559423     1  0.3114      0.961 0.944 0.056
#&gt; GSM559425     1  0.3114      0.961 0.944 0.056
#&gt; GSM559426     1  0.3114      0.961 0.944 0.056
#&gt; GSM559427     1  0.3114      0.961 0.944 0.056
#&gt; GSM559428     1  0.2948      0.962 0.948 0.052
#&gt; GSM559429     1  0.2948      0.962 0.948 0.052
#&gt; GSM559430     1  0.3114      0.961 0.944 0.056
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      0.986 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559432     3  0.3412      0.857 0.000 0.124 0.876
#&gt; GSM559381     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559382     2  0.0747      0.974 0.016 0.984 0.000
#&gt; GSM559384     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559385     1  0.1620      0.958 0.964 0.024 0.012
#&gt; GSM559386     1  0.2537      0.927 0.920 0.080 0.000
#&gt; GSM559388     2  0.0237      0.983 0.004 0.996 0.000
#&gt; GSM559389     1  0.1289      0.959 0.968 0.032 0.000
#&gt; GSM559390     1  0.2537      0.927 0.920 0.080 0.000
#&gt; GSM559392     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559393     1  0.1620      0.958 0.964 0.024 0.012
#&gt; GSM559394     1  0.1620      0.958 0.964 0.024 0.012
#&gt; GSM559396     1  0.5268      0.756 0.776 0.212 0.012
#&gt; GSM559398     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559400     2  0.2229      0.948 0.044 0.944 0.012
#&gt; GSM559402     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559403     1  0.1289      0.959 0.968 0.032 0.000
#&gt; GSM559404     1  0.1289      0.959 0.968 0.032 0.000
#&gt; GSM559405     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559406     1  0.3459      0.908 0.892 0.096 0.012
#&gt; GSM559407     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559411     1  0.2066      0.942 0.940 0.060 0.000
#&gt; GSM559412     1  0.0237      0.967 0.996 0.004 0.000
#&gt; GSM559413     1  0.2066      0.942 0.940 0.060 0.000
#&gt; GSM559415     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM559416     1  0.0892      0.964 0.980 0.020 0.000
#&gt; GSM559417     1  0.1031      0.963 0.976 0.024 0.000
#&gt; GSM559418     1  0.1031      0.963 0.976 0.024 0.000
#&gt; GSM559419     1  0.0424      0.966 0.992 0.008 0.000
#&gt; GSM559420     1  0.0747      0.965 0.984 0.016 0.000
#&gt; GSM559421     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559426     2  0.0237      0.983 0.004 0.996 0.000
#&gt; GSM559427     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM559428     2  0.2116      0.952 0.040 0.948 0.012
#&gt; GSM559429     2  0.2063      0.951 0.044 0.948 0.008
#&gt; GSM559430     2  0.0000      0.984 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559387     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559397     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559401     3  0.4933     -0.626 0.000 0.000 0.568 0.432
#&gt; GSM559414     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559422     4  0.4877      0.991 0.000 0.000 0.408 0.592
#&gt; GSM559424     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM559431     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559432     4  0.4866      0.991 0.000 0.000 0.404 0.596
#&gt; GSM559381     1  0.2973      0.800 0.856 0.000 0.000 0.144
#&gt; GSM559382     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM559384     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM559385     1  0.3486      0.786 0.812 0.000 0.000 0.188
#&gt; GSM559386     1  0.5674      0.743 0.720 0.148 0.000 0.132
#&gt; GSM559388     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559389     1  0.2973      0.800 0.856 0.000 0.000 0.144
#&gt; GSM559390     1  0.4853      0.754 0.744 0.036 0.000 0.220
#&gt; GSM559392     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559393     1  0.4163      0.773 0.792 0.020 0.000 0.188
#&gt; GSM559394     1  0.3486      0.786 0.812 0.000 0.000 0.188
#&gt; GSM559396     1  0.7882      0.401 0.488 0.280 0.012 0.220
#&gt; GSM559398     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559399     1  0.2408      0.809 0.896 0.000 0.000 0.104
#&gt; GSM559400     2  0.3688      0.731 0.000 0.792 0.000 0.208
#&gt; GSM559402     1  0.1211      0.816 0.960 0.000 0.000 0.040
#&gt; GSM559403     1  0.3311      0.793 0.828 0.000 0.000 0.172
#&gt; GSM559404     1  0.3444      0.788 0.816 0.000 0.000 0.184
#&gt; GSM559405     1  0.2973      0.800 0.856 0.000 0.000 0.144
#&gt; GSM559406     1  0.5179      0.740 0.728 0.052 0.000 0.220
#&gt; GSM559407     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM559408     1  0.3649      0.777 0.796 0.000 0.000 0.204
#&gt; GSM559409     1  0.3569      0.780 0.804 0.000 0.000 0.196
#&gt; GSM559410     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM559411     1  0.4364      0.766 0.764 0.016 0.000 0.220
#&gt; GSM559412     1  0.3726      0.774 0.788 0.000 0.000 0.212
#&gt; GSM559413     1  0.3801      0.771 0.780 0.000 0.000 0.220
#&gt; GSM559415     1  0.2973      0.800 0.856 0.000 0.000 0.144
#&gt; GSM559416     1  0.4574      0.763 0.756 0.024 0.000 0.220
#&gt; GSM559417     1  0.4574      0.763 0.756 0.024 0.000 0.220
#&gt; GSM559418     1  0.3441      0.800 0.856 0.024 0.000 0.120
#&gt; GSM559419     1  0.4290      0.770 0.772 0.016 0.000 0.212
#&gt; GSM559420     1  0.1970      0.810 0.932 0.008 0.000 0.060
#&gt; GSM559421     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559423     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559425     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.976 0.000 1.000 0.000 0.000
#&gt; GSM559428     2  0.1389      0.933 0.000 0.952 0.000 0.048
#&gt; GSM559429     2  0.0336      0.971 0.000 0.992 0.000 0.008
#&gt; GSM559430     2  0.0000      0.976 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.0000      0.854 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559387     3  0.0000      0.854 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0404      0.851 0.000 0.000 0.988 0.012 0.000
#&gt; GSM559395     3  0.0000      0.854 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     3  0.0000      0.854 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559401     3  0.5426      0.680 0.000 0.000 0.640 0.252 0.108
#&gt; GSM559414     3  0.0000      0.854 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.6725      0.527 0.000 0.000 0.420 0.288 0.292
#&gt; GSM559424     3  0.0703      0.848 0.000 0.000 0.976 0.024 0.000
#&gt; GSM559431     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     3  0.6779      0.489 0.000 0.000 0.384 0.284 0.332
#&gt; GSM559381     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     2  0.3687      0.821 0.028 0.792 0.000 0.180 0.000
#&gt; GSM559384     1  0.1484      0.792 0.944 0.000 0.000 0.008 0.048
#&gt; GSM559385     5  0.3752      0.935 0.292 0.000 0.000 0.000 0.708
#&gt; GSM559386     1  0.2535      0.713 0.892 0.076 0.000 0.032 0.000
#&gt; GSM559388     2  0.1544      0.893 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559389     1  0.0955      0.794 0.968 0.004 0.000 0.000 0.028
#&gt; GSM559390     4  0.4446      0.909 0.400 0.008 0.000 0.592 0.000
#&gt; GSM559392     2  0.0404      0.909 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559393     5  0.4890      0.866 0.224 0.008 0.000 0.060 0.708
#&gt; GSM559394     5  0.3796      0.936 0.300 0.000 0.000 0.000 0.700
#&gt; GSM559396     4  0.5158      0.793 0.308 0.040 0.012 0.640 0.000
#&gt; GSM559398     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559400     2  0.4455      0.555 0.008 0.588 0.000 0.404 0.000
#&gt; GSM559402     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559403     1  0.4166      0.180 0.648 0.004 0.000 0.000 0.348
#&gt; GSM559404     5  0.4270      0.909 0.320 0.000 0.000 0.012 0.668
#&gt; GSM559405     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.4575      0.903 0.392 0.008 0.000 0.596 0.004
#&gt; GSM559407     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559408     1  0.3039      0.559 0.808 0.000 0.000 0.192 0.000
#&gt; GSM559409     1  0.1608      0.768 0.928 0.000 0.000 0.072 0.000
#&gt; GSM559410     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559411     4  0.4273      0.838 0.448 0.000 0.000 0.552 0.000
#&gt; GSM559412     1  0.3177      0.530 0.792 0.000 0.000 0.208 0.000
#&gt; GSM559413     1  0.3990      0.151 0.688 0.000 0.000 0.308 0.004
#&gt; GSM559415     1  0.0000      0.816 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.4201      0.907 0.408 0.000 0.000 0.592 0.000
#&gt; GSM559417     4  0.4235      0.892 0.424 0.000 0.000 0.576 0.000
#&gt; GSM559418     1  0.1331      0.775 0.952 0.008 0.000 0.040 0.000
#&gt; GSM559419     1  0.4114     -0.232 0.624 0.000 0.000 0.376 0.000
#&gt; GSM559420     1  0.1410      0.766 0.940 0.000 0.000 0.060 0.000
#&gt; GSM559421     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.1478      0.894 0.000 0.936 0.000 0.064 0.000
#&gt; GSM559425     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.4150      0.789 0.036 0.748 0.000 0.216 0.000
#&gt; GSM559429     2  0.4054      0.798 0.036 0.760 0.000 0.204 0.000
#&gt; GSM559430     2  0.0000      0.911 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.0000      0.986 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559387     3  0.0000      0.986 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0865      0.965 0.000 0.000 0.964 0.036 0.000 0.000
#&gt; GSM559395     3  0.0000      0.986 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     3  0.0000      0.986 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559401     5  0.3309      0.859 0.000 0.000 0.280 0.000 0.720 0.000
#&gt; GSM559414     3  0.0000      0.986 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     5  0.2631      0.937 0.000 0.000 0.180 0.000 0.820 0.000
#&gt; GSM559424     3  0.0865      0.965 0.000 0.000 0.964 0.036 0.000 0.000
#&gt; GSM559431     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     5  0.2631      0.937 0.000 0.000 0.180 0.000 0.820 0.000
#&gt; GSM559381     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     2  0.2384      0.898 0.000 0.884 0.000 0.032 0.084 0.000
#&gt; GSM559384     1  0.2066      0.867 0.904 0.000 0.000 0.000 0.024 0.072
#&gt; GSM559385     6  0.0000      0.854 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559386     1  0.1625      0.867 0.928 0.060 0.000 0.012 0.000 0.000
#&gt; GSM559388     2  0.0260      0.952 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559389     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.2384      0.785 0.048 0.000 0.000 0.888 0.064 0.000
#&gt; GSM559392     2  0.0260      0.952 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559393     6  0.0000      0.854 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559394     6  0.0000      0.854 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559396     4  0.1812      0.715 0.008 0.000 0.000 0.912 0.080 0.000
#&gt; GSM559398     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559400     2  0.4200      0.743 0.000 0.740 0.000 0.120 0.140 0.000
#&gt; GSM559402     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559403     6  0.3428      0.531 0.304 0.000 0.000 0.000 0.000 0.696
#&gt; GSM559404     6  0.1267      0.828 0.060 0.000 0.000 0.000 0.000 0.940
#&gt; GSM559405     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.2136      0.769 0.048 0.000 0.000 0.904 0.048 0.000
#&gt; GSM559407     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559408     1  0.3364      0.761 0.780 0.000 0.000 0.196 0.024 0.000
#&gt; GSM559409     1  0.2988      0.810 0.824 0.000 0.000 0.152 0.024 0.000
#&gt; GSM559410     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559411     4  0.3309      0.591 0.280 0.000 0.000 0.720 0.000 0.000
#&gt; GSM559412     1  0.3364      0.761 0.780 0.000 0.000 0.196 0.024 0.000
#&gt; GSM559413     1  0.3266      0.675 0.728 0.000 0.000 0.272 0.000 0.000
#&gt; GSM559415     1  0.0000      0.911 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559416     4  0.2258      0.785 0.044 0.000 0.000 0.896 0.060 0.000
#&gt; GSM559417     4  0.2258      0.785 0.044 0.000 0.000 0.896 0.060 0.000
#&gt; GSM559418     1  0.0622      0.903 0.980 0.000 0.000 0.008 0.000 0.012
#&gt; GSM559419     4  0.3774      0.278 0.408 0.000 0.000 0.592 0.000 0.000
#&gt; GSM559420     1  0.2165      0.853 0.884 0.000 0.000 0.108 0.008 0.000
#&gt; GSM559421     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559423     2  0.0260      0.952 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559425     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     2  0.2537      0.891 0.000 0.872 0.000 0.032 0.096 0.000
#&gt; GSM559429     2  0.2487      0.893 0.000 0.876 0.000 0.032 0.092 0.000
#&gt; GSM559430     2  0.0000      0.953 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:mclust 52         1.99e-10 2
#> MAD:mclust 52         8.27e-11 3
#> MAD:mclust 50         1.37e-09 4
#> MAD:mclust 48         1.40e-08 5
#> MAD:mclust 51         1.14e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.880           0.944       0.973         0.4788 0.527   0.527
#> 3 3 0.969           0.917       0.968         0.3424 0.775   0.593
#> 4 4 0.680           0.625       0.834         0.1157 0.825   0.570
#> 5 5 0.654           0.671       0.795         0.0905 0.839   0.516
#> 6 6 0.671           0.633       0.771         0.0556 0.916   0.654
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.0000      0.964 1.000 0.000
#&gt; GSM559387     1  0.0000      0.964 1.000 0.000
#&gt; GSM559391     1  0.0000      0.964 1.000 0.000
#&gt; GSM559395     1  0.0000      0.964 1.000 0.000
#&gt; GSM559397     1  0.0000      0.964 1.000 0.000
#&gt; GSM559401     1  0.0000      0.964 1.000 0.000
#&gt; GSM559414     1  0.0000      0.964 1.000 0.000
#&gt; GSM559422     1  0.0000      0.964 1.000 0.000
#&gt; GSM559424     1  0.0000      0.964 1.000 0.000
#&gt; GSM559431     2  0.0000      0.983 0.000 1.000
#&gt; GSM559432     2  0.3431      0.922 0.064 0.936
#&gt; GSM559381     1  0.7950      0.721 0.760 0.240
#&gt; GSM559382     2  0.0000      0.983 0.000 1.000
#&gt; GSM559384     1  0.0000      0.964 1.000 0.000
#&gt; GSM559385     1  0.0000      0.964 1.000 0.000
#&gt; GSM559386     2  0.0000      0.983 0.000 1.000
#&gt; GSM559388     2  0.0000      0.983 0.000 1.000
#&gt; GSM559389     1  0.7883      0.727 0.764 0.236
#&gt; GSM559390     1  0.0376      0.963 0.996 0.004
#&gt; GSM559392     2  0.0000      0.983 0.000 1.000
#&gt; GSM559393     2  0.0000      0.983 0.000 1.000
#&gt; GSM559394     1  0.5408      0.864 0.876 0.124
#&gt; GSM559396     1  0.0000      0.964 1.000 0.000
#&gt; GSM559398     2  0.0000      0.983 0.000 1.000
#&gt; GSM559399     1  0.3879      0.909 0.924 0.076
#&gt; GSM559400     2  0.0000      0.983 0.000 1.000
#&gt; GSM559402     1  0.1633      0.950 0.976 0.024
#&gt; GSM559403     1  0.0672      0.961 0.992 0.008
#&gt; GSM559404     1  0.0000      0.964 1.000 0.000
#&gt; GSM559405     1  0.0376      0.963 0.996 0.004
#&gt; GSM559406     1  0.0000      0.964 1.000 0.000
#&gt; GSM559407     1  0.0000      0.964 1.000 0.000
#&gt; GSM559408     1  0.0000      0.964 1.000 0.000
#&gt; GSM559409     1  0.0000      0.964 1.000 0.000
#&gt; GSM559410     1  0.0672      0.961 0.992 0.008
#&gt; GSM559411     1  0.0000      0.964 1.000 0.000
#&gt; GSM559412     1  0.0000      0.964 1.000 0.000
#&gt; GSM559413     1  0.0000      0.964 1.000 0.000
#&gt; GSM559415     1  0.8443      0.668 0.728 0.272
#&gt; GSM559416     1  0.5408      0.858 0.876 0.124
#&gt; GSM559417     2  0.7815      0.687 0.232 0.768
#&gt; GSM559418     2  0.0000      0.983 0.000 1.000
#&gt; GSM559419     1  0.0672      0.961 0.992 0.008
#&gt; GSM559420     1  0.0000      0.964 1.000 0.000
#&gt; GSM559421     2  0.0000      0.983 0.000 1.000
#&gt; GSM559423     2  0.0000      0.983 0.000 1.000
#&gt; GSM559425     2  0.0000      0.983 0.000 1.000
#&gt; GSM559426     2  0.0000      0.983 0.000 1.000
#&gt; GSM559427     2  0.0000      0.983 0.000 1.000
#&gt; GSM559428     2  0.0000      0.983 0.000 1.000
#&gt; GSM559429     2  0.0000      0.983 0.000 1.000
#&gt; GSM559430     2  0.0000      0.983 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559397     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559401     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559422     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559424     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559432     3  0.0000      0.996 0.000 0.000 1.000
#&gt; GSM559381     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559382     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559384     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559385     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559386     2  0.6126      0.380 0.400 0.600 0.000
#&gt; GSM559388     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559389     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559390     1  0.1411      0.944 0.964 0.000 0.036
#&gt; GSM559392     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559393     2  0.6062      0.420 0.384 0.616 0.000
#&gt; GSM559394     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559396     3  0.1411      0.958 0.036 0.000 0.964
#&gt; GSM559398     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559400     2  0.1753      0.876 0.000 0.952 0.048
#&gt; GSM559402     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559404     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559407     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559408     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559409     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559410     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559411     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559412     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559415     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559416     1  0.1411      0.942 0.964 0.036 0.000
#&gt; GSM559417     1  0.6168      0.202 0.588 0.412 0.000
#&gt; GSM559418     2  0.5465      0.608 0.288 0.712 0.000
#&gt; GSM559419     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559428     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559429     2  0.0000      0.915 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      0.915 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     4  0.4543    -0.1049 0.000 0.000 0.324 0.676
#&gt; GSM559387     4  0.4994    -0.5326 0.000 0.000 0.480 0.520
#&gt; GSM559391     4  0.3907     0.1696 0.000 0.000 0.232 0.768
#&gt; GSM559395     3  0.5000     0.4407 0.000 0.000 0.504 0.496
#&gt; GSM559397     3  0.4817     0.5811 0.000 0.000 0.612 0.388
#&gt; GSM559401     3  0.2281     0.6571 0.000 0.000 0.904 0.096
#&gt; GSM559414     3  0.4985     0.4956 0.000 0.000 0.532 0.468
#&gt; GSM559422     3  0.0469     0.6366 0.000 0.000 0.988 0.012
#&gt; GSM559424     4  0.3764     0.2012 0.000 0.000 0.216 0.784
#&gt; GSM559431     2  0.0188     0.9101 0.000 0.996 0.000 0.004
#&gt; GSM559432     3  0.0188     0.6251 0.000 0.004 0.996 0.000
#&gt; GSM559381     1  0.0921     0.8235 0.972 0.000 0.000 0.028
#&gt; GSM559382     2  0.1305     0.9010 0.000 0.960 0.036 0.004
#&gt; GSM559384     1  0.2216     0.7984 0.908 0.000 0.000 0.092
#&gt; GSM559385     1  0.0672     0.8183 0.984 0.000 0.008 0.008
#&gt; GSM559386     2  0.5987     0.0351 0.440 0.520 0.000 0.040
#&gt; GSM559388     2  0.0927     0.9078 0.000 0.976 0.016 0.008
#&gt; GSM559389     1  0.0707     0.8234 0.980 0.000 0.000 0.020
#&gt; GSM559390     4  0.5309     0.3285 0.280 0.028 0.004 0.688
#&gt; GSM559392     2  0.0895     0.9081 0.000 0.976 0.020 0.004
#&gt; GSM559393     1  0.5422     0.6116 0.740 0.040 0.200 0.020
#&gt; GSM559394     1  0.3737     0.7086 0.840 0.004 0.136 0.020
#&gt; GSM559396     4  0.3552     0.2957 0.024 0.000 0.128 0.848
#&gt; GSM559398     2  0.0817     0.9069 0.000 0.976 0.024 0.000
#&gt; GSM559399     1  0.1302     0.8216 0.956 0.000 0.000 0.044
#&gt; GSM559400     2  0.3691     0.8257 0.000 0.856 0.076 0.068
#&gt; GSM559402     1  0.1940     0.8137 0.924 0.000 0.000 0.076
#&gt; GSM559403     1  0.0469     0.8199 0.988 0.000 0.000 0.012
#&gt; GSM559404     1  0.0188     0.8219 0.996 0.000 0.000 0.004
#&gt; GSM559405     1  0.0000     0.8226 1.000 0.000 0.000 0.000
#&gt; GSM559406     1  0.5000     0.2189 0.504 0.000 0.000 0.496
#&gt; GSM559407     1  0.2530     0.8003 0.888 0.000 0.000 0.112
#&gt; GSM559408     1  0.4164     0.6734 0.736 0.000 0.000 0.264
#&gt; GSM559409     1  0.2589     0.7976 0.884 0.000 0.000 0.116
#&gt; GSM559410     1  0.0336     0.8238 0.992 0.000 0.000 0.008
#&gt; GSM559411     4  0.4008     0.4195 0.244 0.000 0.000 0.756
#&gt; GSM559412     1  0.4356     0.6411 0.708 0.000 0.000 0.292
#&gt; GSM559413     1  0.4605     0.5807 0.664 0.000 0.000 0.336
#&gt; GSM559415     1  0.0376     0.8222 0.992 0.004 0.000 0.004
#&gt; GSM559416     4  0.6054     0.3292 0.088 0.256 0.000 0.656
#&gt; GSM559417     2  0.6570     0.3317 0.080 0.572 0.004 0.344
#&gt; GSM559418     1  0.5143     0.4221 0.628 0.360 0.000 0.012
#&gt; GSM559419     4  0.5125     0.0407 0.388 0.008 0.000 0.604
#&gt; GSM559420     1  0.4713     0.4319 0.640 0.000 0.000 0.360
#&gt; GSM559421     2  0.0336     0.9095 0.000 0.992 0.000 0.008
#&gt; GSM559423     2  0.1059     0.9075 0.000 0.972 0.016 0.012
#&gt; GSM559425     2  0.0000     0.9104 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0592     0.9086 0.000 0.984 0.000 0.016
#&gt; GSM559427     2  0.0188     0.9101 0.000 0.996 0.000 0.004
#&gt; GSM559428     2  0.0592     0.9089 0.000 0.984 0.000 0.016
#&gt; GSM559429     2  0.0592     0.9086 0.000 0.984 0.000 0.016
#&gt; GSM559430     2  0.0188     0.9103 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.1571     0.7932 0.000 0.000 0.936 0.060 0.004
#&gt; GSM559387     3  0.1282     0.7911 0.000 0.000 0.952 0.004 0.044
#&gt; GSM559391     3  0.3152     0.7530 0.000 0.000 0.840 0.136 0.024
#&gt; GSM559395     3  0.1282     0.7915 0.000 0.000 0.952 0.004 0.044
#&gt; GSM559397     3  0.2329     0.7073 0.000 0.000 0.876 0.000 0.124
#&gt; GSM559401     5  0.4074     0.5844 0.000 0.000 0.364 0.000 0.636
#&gt; GSM559414     3  0.1908     0.7491 0.000 0.000 0.908 0.000 0.092
#&gt; GSM559422     5  0.3796     0.6613 0.000 0.000 0.300 0.000 0.700
#&gt; GSM559424     3  0.2997     0.7498 0.000 0.000 0.840 0.148 0.012
#&gt; GSM559431     2  0.1741     0.8024 0.000 0.936 0.000 0.024 0.040
#&gt; GSM559432     5  0.3752     0.6629 0.000 0.000 0.292 0.000 0.708
#&gt; GSM559381     1  0.2060     0.7921 0.924 0.008 0.000 0.052 0.016
#&gt; GSM559382     2  0.3003     0.7822 0.000 0.864 0.000 0.044 0.092
#&gt; GSM559384     1  0.5055     0.6769 0.760 0.004 0.068 0.048 0.120
#&gt; GSM559385     1  0.0324     0.7970 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559386     2  0.7233     0.0753 0.384 0.424 0.000 0.136 0.056
#&gt; GSM559388     2  0.4562     0.7337 0.004 0.760 0.000 0.108 0.128
#&gt; GSM559389     1  0.0671     0.7966 0.980 0.004 0.000 0.000 0.016
#&gt; GSM559390     4  0.3947     0.6880 0.036 0.028 0.068 0.844 0.024
#&gt; GSM559392     2  0.3651     0.7721 0.004 0.828 0.000 0.060 0.108
#&gt; GSM559393     1  0.4937     0.6337 0.740 0.060 0.000 0.028 0.172
#&gt; GSM559394     1  0.2612     0.7479 0.868 0.000 0.000 0.008 0.124
#&gt; GSM559396     3  0.5912     0.6020 0.004 0.076 0.700 0.096 0.124
#&gt; GSM559398     2  0.4025     0.7545 0.000 0.792 0.000 0.076 0.132
#&gt; GSM559399     1  0.2554     0.7831 0.896 0.008 0.000 0.076 0.020
#&gt; GSM559400     5  0.6649     0.1903 0.000 0.216 0.004 0.308 0.472
#&gt; GSM559402     1  0.3787     0.7359 0.824 0.008 0.000 0.104 0.064
#&gt; GSM559403     1  0.0162     0.7975 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559404     1  0.1478     0.7820 0.936 0.000 0.000 0.064 0.000
#&gt; GSM559405     1  0.0671     0.7973 0.980 0.000 0.000 0.016 0.004
#&gt; GSM559406     4  0.4668     0.7520 0.168 0.000 0.076 0.748 0.008
#&gt; GSM559407     1  0.5111    -0.2755 0.500 0.000 0.000 0.464 0.036
#&gt; GSM559408     4  0.4152     0.6783 0.296 0.000 0.000 0.692 0.012
#&gt; GSM559409     4  0.4641     0.3795 0.456 0.000 0.000 0.532 0.012
#&gt; GSM559410     1  0.2488     0.7395 0.872 0.000 0.000 0.124 0.004
#&gt; GSM559411     4  0.5974     0.6815 0.108 0.000 0.176 0.668 0.048
#&gt; GSM559412     4  0.4403     0.6707 0.316 0.000 0.004 0.668 0.012
#&gt; GSM559413     4  0.4839     0.6789 0.304 0.000 0.012 0.660 0.024
#&gt; GSM559415     1  0.4204     0.7324 0.808 0.028 0.000 0.104 0.060
#&gt; GSM559416     4  0.2735     0.6522 0.004 0.036 0.056 0.896 0.008
#&gt; GSM559417     4  0.2894     0.6006 0.004 0.084 0.000 0.876 0.036
#&gt; GSM559418     2  0.6571     0.3517 0.348 0.524 0.000 0.068 0.060
#&gt; GSM559419     4  0.5127     0.7435 0.132 0.000 0.096 0.740 0.032
#&gt; GSM559420     1  0.8029     0.2187 0.448 0.004 0.248 0.176 0.124
#&gt; GSM559421     2  0.1915     0.8060 0.000 0.928 0.000 0.032 0.040
#&gt; GSM559423     2  0.3008     0.7726 0.004 0.868 0.000 0.036 0.092
#&gt; GSM559425     2  0.1386     0.8060 0.000 0.952 0.000 0.032 0.016
#&gt; GSM559426     2  0.3030     0.7699 0.004 0.868 0.000 0.040 0.088
#&gt; GSM559427     2  0.3159     0.7842 0.000 0.856 0.000 0.056 0.088
#&gt; GSM559428     2  0.2927     0.7701 0.000 0.868 0.000 0.040 0.092
#&gt; GSM559429     2  0.3161     0.7661 0.004 0.860 0.000 0.044 0.092
#&gt; GSM559430     2  0.1117     0.8056 0.000 0.964 0.000 0.016 0.020
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.1010     0.8801 0.000 0.000 0.960 0.000 0.036 0.004
#&gt; GSM559387     3  0.1910     0.8738 0.000 0.000 0.892 0.000 0.108 0.000
#&gt; GSM559391     3  0.1053     0.8511 0.000 0.000 0.964 0.004 0.020 0.012
#&gt; GSM559395     3  0.1714     0.8789 0.000 0.000 0.908 0.000 0.092 0.000
#&gt; GSM559397     3  0.2838     0.8140 0.000 0.000 0.808 0.000 0.188 0.004
#&gt; GSM559401     5  0.2003     0.9500 0.000 0.000 0.116 0.000 0.884 0.000
#&gt; GSM559414     3  0.2882     0.8193 0.000 0.000 0.812 0.000 0.180 0.008
#&gt; GSM559422     5  0.1444     0.9648 0.000 0.000 0.072 0.000 0.928 0.000
#&gt; GSM559424     3  0.0881     0.8682 0.000 0.000 0.972 0.008 0.012 0.008
#&gt; GSM559431     6  0.3975     0.4363 0.000 0.452 0.000 0.004 0.000 0.544
#&gt; GSM559432     5  0.1753     0.9699 0.000 0.000 0.084 0.000 0.912 0.004
#&gt; GSM559381     1  0.4903     0.6717 0.740 0.036 0.004 0.072 0.012 0.136
#&gt; GSM559382     2  0.5766     0.0416 0.040 0.584 0.000 0.064 0.012 0.300
#&gt; GSM559384     1  0.5888     0.6085 0.636 0.004 0.100 0.044 0.012 0.204
#&gt; GSM559385     1  0.1452     0.7283 0.948 0.020 0.000 0.012 0.000 0.020
#&gt; GSM559386     1  0.7840     0.1044 0.340 0.212 0.000 0.176 0.012 0.260
#&gt; GSM559388     2  0.1691     0.4677 0.028 0.940 0.000 0.012 0.008 0.012
#&gt; GSM559389     1  0.2159     0.7281 0.916 0.040 0.000 0.016 0.004 0.024
#&gt; GSM559390     4  0.3172     0.8163 0.008 0.104 0.008 0.852 0.016 0.012
#&gt; GSM559392     2  0.2051     0.4600 0.004 0.896 0.000 0.000 0.004 0.096
#&gt; GSM559393     1  0.4224     0.5688 0.700 0.256 0.000 0.000 0.036 0.008
#&gt; GSM559394     1  0.2374     0.7192 0.904 0.028 0.000 0.004 0.048 0.016
#&gt; GSM559396     3  0.2730     0.7822 0.004 0.000 0.864 0.004 0.020 0.108
#&gt; GSM559398     2  0.1555     0.4741 0.004 0.932 0.000 0.000 0.004 0.060
#&gt; GSM559399     1  0.5023     0.6150 0.700 0.208 0.008 0.020 0.012 0.052
#&gt; GSM559400     2  0.5843     0.1186 0.000 0.516 0.000 0.220 0.260 0.004
#&gt; GSM559402     1  0.5880     0.4868 0.572 0.000 0.020 0.284 0.012 0.112
#&gt; GSM559403     1  0.0603     0.7269 0.980 0.000 0.000 0.016 0.000 0.004
#&gt; GSM559404     1  0.3997     0.6790 0.776 0.000 0.000 0.108 0.008 0.108
#&gt; GSM559405     1  0.1700     0.7293 0.928 0.000 0.000 0.048 0.000 0.024
#&gt; GSM559406     4  0.3080     0.8467 0.032 0.036 0.020 0.880 0.012 0.020
#&gt; GSM559407     4  0.4893     0.6372 0.204 0.004 0.020 0.712 0.028 0.032
#&gt; GSM559408     4  0.1059     0.8706 0.016 0.016 0.004 0.964 0.000 0.000
#&gt; GSM559409     4  0.3948     0.6888 0.208 0.020 0.000 0.752 0.004 0.016
#&gt; GSM559410     1  0.3277     0.6739 0.792 0.000 0.000 0.188 0.004 0.016
#&gt; GSM559411     4  0.4057     0.8175 0.020 0.000 0.116 0.800 0.032 0.032
#&gt; GSM559412     4  0.1168     0.8662 0.028 0.000 0.000 0.956 0.000 0.016
#&gt; GSM559413     4  0.1793     0.8628 0.036 0.000 0.000 0.928 0.004 0.032
#&gt; GSM559415     1  0.5941     0.6035 0.632 0.040 0.000 0.152 0.016 0.160
#&gt; GSM559416     4  0.2843     0.8474 0.004 0.060 0.040 0.880 0.008 0.008
#&gt; GSM559417     4  0.2151     0.8615 0.004 0.040 0.016 0.920 0.012 0.008
#&gt; GSM559418     2  0.6295     0.0879 0.332 0.456 0.000 0.024 0.000 0.188
#&gt; GSM559419     4  0.3198     0.8497 0.024 0.000 0.060 0.864 0.020 0.032
#&gt; GSM559420     1  0.7827     0.2136 0.320 0.012 0.304 0.092 0.012 0.260
#&gt; GSM559421     2  0.3756    -0.1195 0.000 0.600 0.000 0.000 0.000 0.400
#&gt; GSM559423     6  0.3420     0.7188 0.012 0.240 0.000 0.000 0.000 0.748
#&gt; GSM559425     2  0.3899    -0.1841 0.000 0.592 0.000 0.004 0.000 0.404
#&gt; GSM559426     6  0.3133     0.7615 0.008 0.212 0.000 0.000 0.000 0.780
#&gt; GSM559427     2  0.3052     0.3323 0.000 0.780 0.000 0.004 0.000 0.216
#&gt; GSM559428     6  0.3250     0.7546 0.004 0.196 0.012 0.000 0.000 0.788
#&gt; GSM559429     6  0.3121     0.7475 0.004 0.180 0.012 0.000 0.000 0.804
#&gt; GSM559430     6  0.3866     0.3651 0.000 0.484 0.000 0.000 0.000 0.516
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:NMF 52         2.84e-01 2
#> MAD:NMF 49         4.55e-09 3
#> MAD:NMF 36         7.39e-07 4
#> MAD:NMF 46         2.74e-07 5
#> MAD:NMF 38         9.49e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.788           0.961       0.973         0.4173 0.566   0.566
#> 3 3 0.684           0.818       0.904         0.5718 0.759   0.573
#> 4 4 0.713           0.682       0.827         0.0640 0.989   0.965
#> 5 5 0.736           0.660       0.781         0.0917 0.891   0.671
#> 6 6 0.773           0.710       0.855         0.0338 0.958   0.826
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.0000      0.990 0.000 1.000
#&gt; GSM559387     2  0.0000      0.990 0.000 1.000
#&gt; GSM559391     2  0.0000      0.990 0.000 1.000
#&gt; GSM559395     2  0.0000      0.990 0.000 1.000
#&gt; GSM559397     1  0.0000      0.929 1.000 0.000
#&gt; GSM559401     2  0.0000      0.990 0.000 1.000
#&gt; GSM559414     2  0.0000      0.990 0.000 1.000
#&gt; GSM559422     1  0.6973      0.843 0.812 0.188
#&gt; GSM559424     2  0.0000      0.990 0.000 1.000
#&gt; GSM559431     2  0.0938      0.989 0.012 0.988
#&gt; GSM559432     2  0.0938      0.989 0.012 0.988
#&gt; GSM559381     1  0.0000      0.929 1.000 0.000
#&gt; GSM559382     1  0.0000      0.929 1.000 0.000
#&gt; GSM559384     1  0.0000      0.929 1.000 0.000
#&gt; GSM559385     2  0.0376      0.990 0.004 0.996
#&gt; GSM559386     1  0.0000      0.929 1.000 0.000
#&gt; GSM559388     2  0.1184      0.989 0.016 0.984
#&gt; GSM559389     1  0.0000      0.929 1.000 0.000
#&gt; GSM559390     2  0.2778      0.955 0.048 0.952
#&gt; GSM559392     2  0.1184      0.989 0.016 0.984
#&gt; GSM559393     2  0.0376      0.990 0.004 0.996
#&gt; GSM559394     2  0.0376      0.990 0.004 0.996
#&gt; GSM559396     1  0.4690      0.906 0.900 0.100
#&gt; GSM559398     2  0.0938      0.989 0.012 0.988
#&gt; GSM559399     2  0.0672      0.990 0.008 0.992
#&gt; GSM559400     2  0.1184      0.989 0.016 0.984
#&gt; GSM559402     2  0.3114      0.942 0.056 0.944
#&gt; GSM559403     2  0.0376      0.990 0.004 0.996
#&gt; GSM559404     1  0.6973      0.847 0.812 0.188
#&gt; GSM559405     1  0.0000      0.929 1.000 0.000
#&gt; GSM559406     1  0.4690      0.906 0.900 0.100
#&gt; GSM559407     2  0.0376      0.990 0.004 0.996
#&gt; GSM559408     1  0.4690      0.906 0.900 0.100
#&gt; GSM559409     2  0.0376      0.990 0.004 0.996
#&gt; GSM559410     2  0.0000      0.990 0.000 1.000
#&gt; GSM559411     2  0.0000      0.990 0.000 1.000
#&gt; GSM559412     1  0.6973      0.847 0.812 0.188
#&gt; GSM559413     1  0.6973      0.847 0.812 0.188
#&gt; GSM559415     2  0.0672      0.990 0.008 0.992
#&gt; GSM559416     2  0.0672      0.990 0.008 0.992
#&gt; GSM559417     2  0.0672      0.990 0.008 0.992
#&gt; GSM559418     2  0.0672      0.990 0.008 0.992
#&gt; GSM559419     2  0.1843      0.976 0.028 0.972
#&gt; GSM559420     1  0.0000      0.929 1.000 0.000
#&gt; GSM559421     2  0.1184      0.989 0.016 0.984
#&gt; GSM559423     2  0.1184      0.989 0.016 0.984
#&gt; GSM559425     2  0.0938      0.989 0.012 0.988
#&gt; GSM559426     2  0.0938      0.989 0.012 0.988
#&gt; GSM559427     2  0.0938      0.989 0.012 0.988
#&gt; GSM559428     1  0.0000      0.929 1.000 0.000
#&gt; GSM559429     2  0.1184      0.989 0.016 0.984
#&gt; GSM559430     2  0.0938      0.989 0.012 0.988
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0237      0.925 0.000 0.004 0.996
#&gt; GSM559387     3  0.0000      0.923 0.000 0.000 1.000
#&gt; GSM559391     3  0.0237      0.925 0.000 0.004 0.996
#&gt; GSM559395     3  0.0000      0.923 0.000 0.000 1.000
#&gt; GSM559397     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      0.923 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.923 0.000 0.000 1.000
#&gt; GSM559422     1  0.4399      0.838 0.812 0.000 0.188
#&gt; GSM559424     3  0.1529      0.900 0.000 0.040 0.960
#&gt; GSM559431     2  0.0000      0.814 0.000 1.000 0.000
#&gt; GSM559432     2  0.1964      0.812 0.000 0.944 0.056
#&gt; GSM559381     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559384     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559385     3  0.0661      0.923 0.004 0.008 0.988
#&gt; GSM559386     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559388     2  0.1267      0.820 0.004 0.972 0.024
#&gt; GSM559389     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559390     2  0.7442      0.535 0.044 0.588 0.368
#&gt; GSM559392     2  0.1765      0.822 0.004 0.956 0.040
#&gt; GSM559393     3  0.6033      0.354 0.004 0.336 0.660
#&gt; GSM559394     3  0.6008      0.367 0.004 0.332 0.664
#&gt; GSM559396     1  0.2959      0.903 0.900 0.000 0.100
#&gt; GSM559398     2  0.0000      0.814 0.000 1.000 0.000
#&gt; GSM559399     2  0.6247      0.569 0.004 0.620 0.376
#&gt; GSM559400     2  0.2301      0.818 0.004 0.936 0.060
#&gt; GSM559402     3  0.4458      0.818 0.056 0.080 0.864
#&gt; GSM559403     3  0.0661      0.923 0.004 0.008 0.988
#&gt; GSM559404     1  0.4399      0.839 0.812 0.000 0.188
#&gt; GSM559405     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559406     1  0.2959      0.903 0.900 0.000 0.100
#&gt; GSM559407     3  0.0661      0.923 0.004 0.008 0.988
#&gt; GSM559408     1  0.2959      0.903 0.900 0.000 0.100
#&gt; GSM559409     3  0.0829      0.921 0.004 0.012 0.984
#&gt; GSM559410     3  0.0000      0.923 0.000 0.000 1.000
#&gt; GSM559411     3  0.0237      0.925 0.000 0.004 0.996
#&gt; GSM559412     1  0.4399      0.839 0.812 0.000 0.188
#&gt; GSM559413     1  0.4399      0.839 0.812 0.000 0.188
#&gt; GSM559415     2  0.6247      0.569 0.004 0.620 0.376
#&gt; GSM559416     2  0.6209      0.582 0.004 0.628 0.368
#&gt; GSM559417     2  0.6081      0.612 0.004 0.652 0.344
#&gt; GSM559418     2  0.6209      0.582 0.004 0.628 0.368
#&gt; GSM559419     2  0.6899      0.569 0.024 0.612 0.364
#&gt; GSM559420     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559421     2  0.1765      0.822 0.004 0.956 0.040
#&gt; GSM559423     2  0.1765      0.822 0.004 0.956 0.040
#&gt; GSM559425     2  0.0000      0.814 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.814 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.814 0.000 1.000 0.000
#&gt; GSM559428     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM559429     2  0.3112      0.803 0.004 0.900 0.096
#&gt; GSM559430     2  0.0000      0.814 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.0895      0.872 0.020 0.004 0.976 0.000
#&gt; GSM559387     3  0.2149      0.843 0.000 0.000 0.912 0.088
#&gt; GSM559391     3  0.0188      0.876 0.000 0.004 0.996 0.000
#&gt; GSM559395     3  0.2149      0.843 0.000 0.000 0.912 0.088
#&gt; GSM559397     1  0.3942      0.576 0.764 0.000 0.000 0.236
#&gt; GSM559401     3  0.2149      0.843 0.000 0.000 0.912 0.088
#&gt; GSM559414     3  0.2149      0.843 0.000 0.000 0.912 0.088
#&gt; GSM559422     1  0.2334      0.568 0.908 0.004 0.088 0.000
#&gt; GSM559424     3  0.1302      0.861 0.000 0.044 0.956 0.000
#&gt; GSM559431     2  0.1474      0.789 0.000 0.948 0.000 0.052
#&gt; GSM559432     2  0.3009      0.792 0.000 0.892 0.056 0.052
#&gt; GSM559381     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559382     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559384     1  0.4431      0.566 0.696 0.000 0.000 0.304
#&gt; GSM559385     3  0.0524      0.877 0.004 0.008 0.988 0.000
#&gt; GSM559386     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559388     2  0.2383      0.799 0.004 0.924 0.024 0.048
#&gt; GSM559389     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559390     2  0.6022      0.562 0.048 0.612 0.336 0.004
#&gt; GSM559392     2  0.0844      0.801 0.004 0.980 0.012 0.004
#&gt; GSM559393     3  0.5064      0.304 0.004 0.360 0.632 0.004
#&gt; GSM559394     3  0.5030      0.321 0.004 0.352 0.640 0.004
#&gt; GSM559396     1  0.2401      0.607 0.904 0.004 0.000 0.092
#&gt; GSM559398     2  0.1474      0.789 0.000 0.948 0.000 0.052
#&gt; GSM559399     2  0.5013      0.590 0.004 0.644 0.348 0.004
#&gt; GSM559400     2  0.1863      0.803 0.004 0.944 0.040 0.012
#&gt; GSM559402     3  0.4535      0.758 0.080 0.104 0.812 0.004
#&gt; GSM559403     3  0.0524      0.877 0.004 0.008 0.988 0.000
#&gt; GSM559404     1  0.2216      0.569 0.908 0.000 0.092 0.000
#&gt; GSM559405     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559406     1  0.0188      0.604 0.996 0.004 0.000 0.000
#&gt; GSM559407     3  0.0524      0.877 0.004 0.008 0.988 0.000
#&gt; GSM559408     1  0.0188      0.604 0.996 0.004 0.000 0.000
#&gt; GSM559409     3  0.1284      0.869 0.024 0.012 0.964 0.000
#&gt; GSM559410     3  0.2149      0.843 0.000 0.000 0.912 0.088
#&gt; GSM559411     3  0.0188      0.876 0.000 0.004 0.996 0.000
#&gt; GSM559412     1  0.2216      0.569 0.908 0.000 0.092 0.000
#&gt; GSM559413     1  0.2216      0.569 0.908 0.000 0.092 0.000
#&gt; GSM559415     2  0.5030      0.585 0.004 0.640 0.352 0.004
#&gt; GSM559416     2  0.4995      0.598 0.004 0.648 0.344 0.004
#&gt; GSM559417     2  0.4854      0.629 0.004 0.676 0.316 0.004
#&gt; GSM559418     2  0.4976      0.601 0.004 0.652 0.340 0.004
#&gt; GSM559419     2  0.5525      0.590 0.024 0.636 0.336 0.004
#&gt; GSM559420     1  0.4843      0.528 0.604 0.000 0.000 0.396
#&gt; GSM559421     2  0.0844      0.801 0.004 0.980 0.012 0.004
#&gt; GSM559423     2  0.0844      0.801 0.004 0.980 0.012 0.004
#&gt; GSM559425     2  0.1474      0.789 0.000 0.948 0.000 0.052
#&gt; GSM559426     2  0.1474      0.789 0.000 0.948 0.000 0.052
#&gt; GSM559427     2  0.1474      0.789 0.000 0.948 0.000 0.052
#&gt; GSM559428     4  0.2973      0.000 0.144 0.000 0.000 0.856
#&gt; GSM559429     2  0.1978      0.794 0.004 0.928 0.068 0.000
#&gt; GSM559430     2  0.1474      0.789 0.000 0.948 0.000 0.052
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.4181     0.8235 0.000 0.000 0.712 0.268 0.020
#&gt; GSM559387     3  0.0771     0.7409 0.000 0.004 0.976 0.020 0.000
#&gt; GSM559391     3  0.3612     0.8293 0.000 0.000 0.732 0.268 0.000
#&gt; GSM559395     3  0.0771     0.7409 0.000 0.004 0.976 0.020 0.000
#&gt; GSM559397     1  0.2732     0.5790 0.840 0.000 0.000 0.000 0.160
#&gt; GSM559401     3  0.0771     0.7409 0.000 0.004 0.976 0.020 0.000
#&gt; GSM559414     3  0.0162     0.7214 0.000 0.004 0.996 0.000 0.000
#&gt; GSM559422     1  0.5818     0.5750 0.460 0.000 0.000 0.092 0.448
#&gt; GSM559424     3  0.3857     0.7929 0.000 0.000 0.688 0.312 0.000
#&gt; GSM559431     2  0.0162     0.9826 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559432     2  0.1628     0.8982 0.000 0.936 0.008 0.056 0.000
#&gt; GSM559381     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559384     1  0.2329     0.5830 0.876 0.000 0.000 0.000 0.124
#&gt; GSM559385     3  0.3661     0.8278 0.000 0.000 0.724 0.276 0.000
#&gt; GSM559386     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559388     4  0.4283     0.2632 0.000 0.456 0.000 0.544 0.000
#&gt; GSM559389     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.2760     0.6828 0.028 0.000 0.064 0.892 0.016
#&gt; GSM559392     4  0.3586     0.5706 0.000 0.264 0.000 0.736 0.000
#&gt; GSM559393     4  0.4045     0.0605 0.000 0.000 0.356 0.644 0.000
#&gt; GSM559394     4  0.4074     0.0330 0.000 0.000 0.364 0.636 0.000
#&gt; GSM559396     1  0.4298     0.6138 0.640 0.000 0.000 0.008 0.352
#&gt; GSM559398     2  0.0290     0.9790 0.000 0.992 0.000 0.008 0.000
#&gt; GSM559399     4  0.1608     0.7000 0.000 0.000 0.072 0.928 0.000
#&gt; GSM559400     4  0.3814     0.5693 0.000 0.276 0.004 0.720 0.000
#&gt; GSM559402     3  0.5879     0.6009 0.028 0.000 0.540 0.384 0.048
#&gt; GSM559403     3  0.3661     0.8278 0.000 0.000 0.724 0.276 0.000
#&gt; GSM559404     1  0.5921     0.5730 0.456 0.000 0.004 0.088 0.452
#&gt; GSM559405     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     1  0.4528     0.6091 0.548 0.000 0.000 0.008 0.444
#&gt; GSM559407     3  0.3661     0.8278 0.000 0.000 0.724 0.276 0.000
#&gt; GSM559408     1  0.4528     0.6091 0.548 0.000 0.000 0.008 0.444
#&gt; GSM559409     3  0.4252     0.8165 0.000 0.000 0.700 0.280 0.020
#&gt; GSM559410     3  0.0162     0.7214 0.000 0.004 0.996 0.000 0.000
#&gt; GSM559411     3  0.3612     0.8293 0.000 0.000 0.732 0.268 0.000
#&gt; GSM559412     1  0.5921     0.5730 0.456 0.000 0.004 0.088 0.452
#&gt; GSM559413     1  0.5921     0.5730 0.456 0.000 0.004 0.088 0.452
#&gt; GSM559415     4  0.1671     0.6972 0.000 0.000 0.076 0.924 0.000
#&gt; GSM559416     4  0.1544     0.7028 0.000 0.000 0.068 0.932 0.000
#&gt; GSM559417     4  0.1043     0.7081 0.000 0.000 0.040 0.960 0.000
#&gt; GSM559418     4  0.1478     0.7042 0.000 0.000 0.064 0.936 0.000
#&gt; GSM559419     4  0.2141     0.6986 0.016 0.000 0.064 0.916 0.004
#&gt; GSM559420     1  0.0000     0.5414 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559421     4  0.3586     0.5706 0.000 0.264 0.000 0.736 0.000
#&gt; GSM559423     4  0.3586     0.5706 0.000 0.264 0.000 0.736 0.000
#&gt; GSM559425     2  0.0162     0.9826 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559426     2  0.0162     0.9826 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559427     2  0.0162     0.9826 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559428     5  0.4278     0.0000 0.452 0.000 0.000 0.000 0.548
#&gt; GSM559429     4  0.4167     0.6066 0.000 0.252 0.024 0.724 0.000
#&gt; GSM559430     2  0.0162     0.9826 0.000 0.996 0.000 0.004 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.3834     0.8214 0.268 0.000 0.708 0.024 0.000 0.000
#&gt; GSM559387     3  0.0951     0.7173 0.020 0.000 0.968 0.008 0.004 0.000
#&gt; GSM559391     3  0.3383     0.8266 0.268 0.000 0.728 0.004 0.000 0.000
#&gt; GSM559395     3  0.0951     0.7173 0.020 0.000 0.968 0.008 0.004 0.000
#&gt; GSM559397     6  0.3126     0.5634 0.000 0.000 0.000 0.248 0.000 0.752
#&gt; GSM559401     3  0.0951     0.7173 0.020 0.000 0.968 0.008 0.004 0.000
#&gt; GSM559414     3  0.0405     0.6957 0.000 0.000 0.988 0.008 0.004 0.000
#&gt; GSM559422     4  0.0508     0.7118 0.004 0.000 0.000 0.984 0.000 0.012
#&gt; GSM559424     3  0.3601     0.7902 0.312 0.000 0.684 0.004 0.000 0.000
#&gt; GSM559431     2  0.0000     0.9813 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     2  0.1434     0.8896 0.048 0.940 0.012 0.000 0.000 0.000
#&gt; GSM559381     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559382     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559384     6  0.2092     0.7911 0.000 0.000 0.000 0.124 0.000 0.876
#&gt; GSM559385     3  0.3426     0.8251 0.276 0.000 0.720 0.004 0.000 0.000
#&gt; GSM559386     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559388     1  0.3847     0.2715 0.544 0.456 0.000 0.000 0.000 0.000
#&gt; GSM559389     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559390     1  0.2258     0.6873 0.896 0.000 0.060 0.044 0.000 0.000
#&gt; GSM559392     1  0.3360     0.5754 0.732 0.264 0.000 0.000 0.004 0.000
#&gt; GSM559393     1  0.3756     0.0600 0.644 0.000 0.352 0.004 0.000 0.000
#&gt; GSM559394     1  0.3782     0.0326 0.636 0.000 0.360 0.004 0.000 0.000
#&gt; GSM559396     4  0.4072     0.3694 0.008 0.000 0.000 0.544 0.000 0.448
#&gt; GSM559398     2  0.0146     0.9773 0.004 0.996 0.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.1387     0.7019 0.932 0.000 0.068 0.000 0.000 0.000
#&gt; GSM559400     1  0.3288     0.5738 0.724 0.276 0.000 0.000 0.000 0.000
#&gt; GSM559402     3  0.5044     0.6018 0.384 0.000 0.536 0.080 0.000 0.000
#&gt; GSM559403     3  0.3426     0.8251 0.276 0.000 0.720 0.004 0.000 0.000
#&gt; GSM559404     4  0.0260     0.7120 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM559405     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559406     4  0.3861     0.5609 0.008 0.000 0.000 0.640 0.000 0.352
#&gt; GSM559407     3  0.3426     0.8251 0.276 0.000 0.720 0.004 0.000 0.000
#&gt; GSM559408     4  0.3861     0.5609 0.008 0.000 0.000 0.640 0.000 0.352
#&gt; GSM559409     3  0.3897     0.8144 0.280 0.000 0.696 0.024 0.000 0.000
#&gt; GSM559410     3  0.0405     0.6957 0.000 0.000 0.988 0.008 0.004 0.000
#&gt; GSM559411     3  0.3383     0.8266 0.268 0.000 0.728 0.004 0.000 0.000
#&gt; GSM559412     4  0.0260     0.7120 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM559413     4  0.0260     0.7120 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM559415     1  0.1444     0.6991 0.928 0.000 0.072 0.000 0.000 0.000
#&gt; GSM559416     1  0.1327     0.7046 0.936 0.000 0.064 0.000 0.000 0.000
#&gt; GSM559417     1  0.0865     0.7098 0.964 0.000 0.036 0.000 0.000 0.000
#&gt; GSM559418     1  0.1267     0.7061 0.940 0.000 0.060 0.000 0.000 0.000
#&gt; GSM559419     1  0.1807     0.7016 0.920 0.000 0.060 0.020 0.000 0.000
#&gt; GSM559420     6  0.0000     0.9289 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559421     1  0.3360     0.5754 0.732 0.264 0.000 0.000 0.004 0.000
#&gt; GSM559423     1  0.3360     0.5754 0.732 0.264 0.000 0.000 0.004 0.000
#&gt; GSM559425     2  0.0000     0.9813 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0000     0.9813 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000     0.9813 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     5  0.0260     0.0000 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM559429     1  0.3665     0.6109 0.728 0.252 0.020 0.000 0.000 0.000
#&gt; GSM559430     2  0.0000     0.9813 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:hclust 52           0.5152 2
#> ATC:hclust 50           0.0116 3
#> ATC:hclust 49           0.0136 4
#> ATC:hclust 48           0.0163 5
#> ATC:hclust 47           0.0408 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.479           0.832       0.843         0.4186 0.566   0.566
#> 3 3 1.000           0.998       0.995         0.5689 0.775   0.601
#> 4 4 0.694           0.741       0.776         0.1151 0.889   0.680
#> 5 5 0.770           0.675       0.784         0.0658 0.804   0.413
#> 6 6 0.764           0.789       0.839         0.0379 0.941   0.751
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2  0.0000      0.791 0.000 1.000
#&gt; GSM559387     2  0.0000      0.791 0.000 1.000
#&gt; GSM559391     2  0.0000      0.791 0.000 1.000
#&gt; GSM559395     2  0.0376      0.791 0.004 0.996
#&gt; GSM559397     1  0.9358      0.999 0.648 0.352
#&gt; GSM559401     2  0.0000      0.791 0.000 1.000
#&gt; GSM559414     2  0.0000      0.791 0.000 1.000
#&gt; GSM559422     1  0.9358      0.999 0.648 0.352
#&gt; GSM559424     2  0.0000      0.791 0.000 1.000
#&gt; GSM559431     2  0.9427      0.705 0.360 0.640
#&gt; GSM559432     2  0.9358      0.702 0.352 0.648
#&gt; GSM559381     1  0.9358      0.999 0.648 0.352
#&gt; GSM559382     1  0.9358      0.999 0.648 0.352
#&gt; GSM559384     1  0.9358      0.999 0.648 0.352
#&gt; GSM559385     2  0.0000      0.791 0.000 1.000
#&gt; GSM559386     1  0.9358      0.999 0.648 0.352
#&gt; GSM559388     2  0.9427      0.705 0.360 0.640
#&gt; GSM559389     1  0.9358      0.999 0.648 0.352
#&gt; GSM559390     2  0.0672      0.788 0.008 0.992
#&gt; GSM559392     2  0.9427      0.705 0.360 0.640
#&gt; GSM559393     2  0.0672      0.788 0.008 0.992
#&gt; GSM559394     2  0.0000      0.791 0.000 1.000
#&gt; GSM559396     1  0.9358      0.999 0.648 0.352
#&gt; GSM559398     2  0.9427      0.705 0.360 0.640
#&gt; GSM559399     2  0.0672      0.788 0.008 0.992
#&gt; GSM559400     2  0.9427      0.705 0.360 0.640
#&gt; GSM559402     2  0.0672      0.788 0.008 0.992
#&gt; GSM559403     2  0.0000      0.791 0.000 1.000
#&gt; GSM559404     1  0.9427      0.988 0.640 0.360
#&gt; GSM559405     1  0.9358      0.999 0.648 0.352
#&gt; GSM559406     1  0.9358      0.999 0.648 0.352
#&gt; GSM559407     2  0.0000      0.791 0.000 1.000
#&gt; GSM559408     1  0.9358      0.999 0.648 0.352
#&gt; GSM559409     2  0.0000      0.791 0.000 1.000
#&gt; GSM559410     2  0.0000      0.791 0.000 1.000
#&gt; GSM559411     2  0.0000      0.791 0.000 1.000
#&gt; GSM559412     1  0.9358      0.999 0.648 0.352
#&gt; GSM559413     1  0.9358      0.999 0.648 0.352
#&gt; GSM559415     2  0.0938      0.789 0.012 0.988
#&gt; GSM559416     2  0.4939      0.765 0.108 0.892
#&gt; GSM559417     2  0.0938      0.789 0.012 0.988
#&gt; GSM559418     2  0.0672      0.788 0.008 0.992
#&gt; GSM559419     2  0.0672      0.788 0.008 0.992
#&gt; GSM559420     1  0.9358      0.999 0.648 0.352
#&gt; GSM559421     2  0.9427      0.705 0.360 0.640
#&gt; GSM559423     2  0.9358      0.706 0.352 0.648
#&gt; GSM559425     2  0.9427      0.705 0.360 0.640
#&gt; GSM559426     2  0.9427      0.705 0.360 0.640
#&gt; GSM559427     2  0.9427      0.705 0.360 0.640
#&gt; GSM559428     1  0.9358      0.999 0.648 0.352
#&gt; GSM559429     2  0.9427      0.705 0.360 0.640
#&gt; GSM559430     2  0.9427      0.705 0.360 0.640
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559397     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559422     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559424     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559431     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559432     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559381     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559384     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559385     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559386     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559388     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559389     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559390     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559392     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559393     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559394     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559396     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559398     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559399     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559400     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559402     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559403     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559404     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559407     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559408     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559409     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559410     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559411     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559412     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559415     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559416     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559417     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559418     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559419     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559420     1  0.0000      0.999 1.000 0.000 0.000
#&gt; GSM559421     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559423     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559425     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559426     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559427     2  0.0747      0.997 0.000 0.984 0.016
#&gt; GSM559428     1  0.0747      0.988 0.984 0.016 0.000
#&gt; GSM559429     2  0.1860      0.959 0.000 0.948 0.052
#&gt; GSM559430     2  0.0747      0.997 0.000 0.984 0.016
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.4331      0.649 0.000 0.000 0.712 0.288
#&gt; GSM559387     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559397     1  0.3311      0.836 0.828 0.000 0.000 0.172
#&gt; GSM559401     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559422     1  0.4898      0.774 0.584 0.000 0.000 0.416
#&gt; GSM559424     3  0.4222      0.671 0.000 0.000 0.728 0.272
#&gt; GSM559431     2  0.0188      0.830 0.000 0.996 0.000 0.004
#&gt; GSM559432     2  0.0188      0.830 0.000 0.996 0.000 0.004
#&gt; GSM559381     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559384     1  0.3610      0.834 0.800 0.000 0.000 0.200
#&gt; GSM559385     3  0.4250      0.666 0.000 0.000 0.724 0.276
#&gt; GSM559386     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559388     2  0.3444      0.756 0.000 0.816 0.000 0.184
#&gt; GSM559389     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.4164      0.797 0.000 0.000 0.264 0.736
#&gt; GSM559392     2  0.4790      0.590 0.000 0.620 0.000 0.380
#&gt; GSM559393     4  0.4624      0.780 0.000 0.000 0.340 0.660
#&gt; GSM559394     3  0.4222      0.671 0.000 0.000 0.728 0.272
#&gt; GSM559396     1  0.4585      0.813 0.668 0.000 0.000 0.332
#&gt; GSM559398     2  0.0000      0.830 0.000 1.000 0.000 0.000
#&gt; GSM559399     4  0.4730      0.753 0.000 0.000 0.364 0.636
#&gt; GSM559400     2  0.4941      0.496 0.000 0.564 0.000 0.436
#&gt; GSM559402     4  0.4250      0.789 0.000 0.000 0.276 0.724
#&gt; GSM559403     3  0.4250      0.666 0.000 0.000 0.724 0.276
#&gt; GSM559404     1  0.4697      0.811 0.644 0.000 0.000 0.356
#&gt; GSM559405     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559406     1  0.4898      0.774 0.584 0.000 0.000 0.416
#&gt; GSM559407     3  0.4222      0.671 0.000 0.000 0.728 0.272
#&gt; GSM559408     1  0.4916      0.767 0.576 0.000 0.000 0.424
#&gt; GSM559409     3  0.4331      0.646 0.000 0.000 0.712 0.288
#&gt; GSM559410     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559411     3  0.0000      0.755 0.000 0.000 1.000 0.000
#&gt; GSM559412     1  0.4697      0.811 0.644 0.000 0.000 0.356
#&gt; GSM559413     1  0.4697      0.811 0.644 0.000 0.000 0.356
#&gt; GSM559415     4  0.4761      0.743 0.000 0.000 0.372 0.628
#&gt; GSM559416     4  0.4761      0.743 0.000 0.000 0.372 0.628
#&gt; GSM559417     4  0.4222      0.807 0.000 0.000 0.272 0.728
#&gt; GSM559418     4  0.4331      0.809 0.000 0.000 0.288 0.712
#&gt; GSM559419     4  0.4277      0.809 0.000 0.000 0.280 0.720
#&gt; GSM559420     1  0.0000      0.828 1.000 0.000 0.000 0.000
#&gt; GSM559421     2  0.4356      0.681 0.000 0.708 0.000 0.292
#&gt; GSM559423     2  0.4961      0.477 0.000 0.552 0.000 0.448
#&gt; GSM559425     2  0.0000      0.830 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0188      0.830 0.000 0.996 0.000 0.004
#&gt; GSM559427     2  0.0000      0.830 0.000 1.000 0.000 0.000
#&gt; GSM559428     1  0.1302      0.806 0.956 0.000 0.000 0.044
#&gt; GSM559429     4  0.5112     -0.255 0.000 0.436 0.004 0.560
#&gt; GSM559430     2  0.0188      0.830 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     1  0.5348     0.1736 0.492 0.000 0.456 0.052 0.000
#&gt; GSM559387     3  0.1357     0.9873 0.048 0.000 0.948 0.000 0.004
#&gt; GSM559391     3  0.1544     0.9727 0.068 0.000 0.932 0.000 0.000
#&gt; GSM559395     3  0.1357     0.9873 0.048 0.000 0.948 0.000 0.004
#&gt; GSM559397     4  0.3333     0.4681 0.000 0.000 0.004 0.788 0.208
#&gt; GSM559401     3  0.1357     0.9873 0.048 0.000 0.948 0.000 0.004
#&gt; GSM559414     3  0.1357     0.9873 0.048 0.000 0.948 0.000 0.004
#&gt; GSM559422     4  0.1124     0.8759 0.036 0.000 0.004 0.960 0.000
#&gt; GSM559424     1  0.4278     0.2448 0.548 0.000 0.452 0.000 0.000
#&gt; GSM559431     2  0.0404     0.8401 0.000 0.988 0.012 0.000 0.000
#&gt; GSM559432     2  0.0703     0.8365 0.000 0.976 0.024 0.000 0.000
#&gt; GSM559381     5  0.4192     0.9536 0.000 0.000 0.000 0.404 0.596
#&gt; GSM559382     5  0.4126     0.9435 0.000 0.000 0.000 0.380 0.620
#&gt; GSM559384     4  0.2230     0.7478 0.000 0.000 0.000 0.884 0.116
#&gt; GSM559385     1  0.4890     0.2267 0.524 0.000 0.452 0.024 0.000
#&gt; GSM559386     5  0.4138     0.9457 0.000 0.000 0.000 0.384 0.616
#&gt; GSM559388     2  0.6127     0.5723 0.220 0.584 0.004 0.000 0.192
#&gt; GSM559389     5  0.4192     0.9536 0.000 0.000 0.000 0.404 0.596
#&gt; GSM559390     1  0.0794     0.6495 0.972 0.000 0.000 0.028 0.000
#&gt; GSM559392     1  0.6896    -0.1870 0.436 0.252 0.000 0.008 0.304
#&gt; GSM559393     1  0.0451     0.6519 0.988 0.000 0.008 0.004 0.000
#&gt; GSM559394     1  0.4262     0.2631 0.560 0.000 0.440 0.000 0.000
#&gt; GSM559396     4  0.2450     0.8242 0.028 0.000 0.000 0.896 0.076
#&gt; GSM559398     2  0.3662     0.7463 0.004 0.744 0.000 0.000 0.252
#&gt; GSM559399     1  0.0566     0.6514 0.984 0.000 0.012 0.004 0.000
#&gt; GSM559400     1  0.6408     0.0704 0.544 0.180 0.000 0.008 0.268
#&gt; GSM559402     1  0.3969     0.4595 0.692 0.000 0.004 0.304 0.000
#&gt; GSM559403     1  0.4890     0.2267 0.524 0.000 0.452 0.024 0.000
#&gt; GSM559404     4  0.0854     0.8842 0.012 0.000 0.004 0.976 0.008
#&gt; GSM559405     5  0.4192     0.9536 0.000 0.000 0.000 0.404 0.596
#&gt; GSM559406     4  0.0963     0.8765 0.036 0.000 0.000 0.964 0.000
#&gt; GSM559407     1  0.4890     0.2267 0.524 0.000 0.452 0.024 0.000
#&gt; GSM559408     4  0.0963     0.8765 0.036 0.000 0.000 0.964 0.000
#&gt; GSM559409     1  0.5161     0.2223 0.516 0.000 0.444 0.040 0.000
#&gt; GSM559410     3  0.1981     0.9783 0.048 0.000 0.924 0.000 0.028
#&gt; GSM559411     3  0.1764     0.9752 0.064 0.000 0.928 0.000 0.008
#&gt; GSM559412     4  0.0854     0.8842 0.012 0.000 0.004 0.976 0.008
#&gt; GSM559413     4  0.0854     0.8842 0.012 0.000 0.004 0.976 0.008
#&gt; GSM559415     1  0.0609     0.6494 0.980 0.000 0.020 0.000 0.000
#&gt; GSM559416     1  0.0771     0.6500 0.976 0.000 0.020 0.000 0.004
#&gt; GSM559417     1  0.0771     0.6515 0.976 0.000 0.000 0.020 0.004
#&gt; GSM559418     1  0.0771     0.6515 0.976 0.000 0.000 0.020 0.004
#&gt; GSM559419     1  0.0771     0.6515 0.976 0.000 0.000 0.020 0.004
#&gt; GSM559420     5  0.4192     0.9536 0.000 0.000 0.000 0.404 0.596
#&gt; GSM559421     2  0.6791     0.2734 0.356 0.360 0.000 0.000 0.284
#&gt; GSM559423     1  0.6462     0.0492 0.528 0.176 0.000 0.008 0.288
#&gt; GSM559425     2  0.0865     0.8389 0.004 0.972 0.000 0.000 0.024
#&gt; GSM559426     2  0.0510     0.8400 0.000 0.984 0.016 0.000 0.000
#&gt; GSM559427     2  0.0865     0.8389 0.004 0.972 0.000 0.000 0.024
#&gt; GSM559428     5  0.4773     0.8231 0.008 0.000 0.024 0.312 0.656
#&gt; GSM559429     1  0.3488     0.5523 0.860 0.072 0.016 0.008 0.044
#&gt; GSM559430     2  0.0404     0.8401 0.000 0.988 0.012 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     1  0.5238      0.578 0.588 0.020 0.324 0.068 0.000 0.000
#&gt; GSM559387     3  0.0146      0.971 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM559391     3  0.1562      0.942 0.032 0.004 0.940 0.024 0.000 0.000
#&gt; GSM559395     3  0.0146      0.971 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM559397     4  0.4958      0.659 0.000 0.076 0.000 0.560 0.000 0.364
#&gt; GSM559401     3  0.0436      0.971 0.004 0.004 0.988 0.004 0.000 0.000
#&gt; GSM559414     3  0.0436      0.971 0.004 0.004 0.988 0.004 0.000 0.000
#&gt; GSM559422     4  0.5153      0.848 0.028 0.116 0.000 0.676 0.000 0.180
#&gt; GSM559424     1  0.4373      0.615 0.624 0.004 0.344 0.028 0.000 0.000
#&gt; GSM559431     5  0.0000      0.923 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559432     5  0.1138      0.909 0.000 0.012 0.004 0.024 0.960 0.000
#&gt; GSM559381     6  0.0363      0.951 0.000 0.000 0.000 0.012 0.000 0.988
#&gt; GSM559382     6  0.0260      0.949 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; GSM559384     4  0.3592      0.769 0.000 0.000 0.000 0.656 0.000 0.344
#&gt; GSM559385     1  0.4479      0.619 0.624 0.004 0.336 0.036 0.000 0.000
#&gt; GSM559386     6  0.0260      0.949 0.000 0.008 0.000 0.000 0.000 0.992
#&gt; GSM559388     2  0.6305      0.438 0.180 0.408 0.004 0.016 0.392 0.000
#&gt; GSM559389     6  0.0363      0.951 0.000 0.000 0.000 0.012 0.000 0.988
#&gt; GSM559390     1  0.1245      0.726 0.952 0.032 0.000 0.016 0.000 0.000
#&gt; GSM559392     2  0.4382      0.767 0.200 0.716 0.000 0.000 0.080 0.004
#&gt; GSM559393     1  0.0000      0.732 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559394     1  0.4093      0.659 0.680 0.004 0.292 0.024 0.000 0.000
#&gt; GSM559396     4  0.4767      0.797 0.020 0.036 0.000 0.628 0.000 0.316
#&gt; GSM559398     2  0.4508      0.243 0.000 0.568 0.000 0.036 0.396 0.000
#&gt; GSM559399     1  0.0603      0.735 0.980 0.004 0.016 0.000 0.000 0.000
#&gt; GSM559400     2  0.4601      0.729 0.312 0.628 0.000 0.000 0.060 0.000
#&gt; GSM559402     1  0.3248      0.662 0.804 0.032 0.000 0.164 0.000 0.000
#&gt; GSM559403     1  0.4479      0.619 0.624 0.004 0.336 0.036 0.000 0.000
#&gt; GSM559404     4  0.3198      0.880 0.008 0.008 0.000 0.796 0.000 0.188
#&gt; GSM559405     6  0.0713      0.939 0.000 0.000 0.000 0.028 0.000 0.972
#&gt; GSM559406     4  0.4130      0.870 0.028 0.036 0.000 0.756 0.000 0.180
#&gt; GSM559407     1  0.4479      0.619 0.624 0.004 0.336 0.036 0.000 0.000
#&gt; GSM559408     4  0.4130      0.870 0.028 0.036 0.000 0.756 0.000 0.180
#&gt; GSM559409     1  0.4809      0.635 0.640 0.020 0.296 0.044 0.000 0.000
#&gt; GSM559410     3  0.1565      0.950 0.004 0.028 0.940 0.028 0.000 0.000
#&gt; GSM559411     3  0.1485      0.949 0.024 0.004 0.944 0.028 0.000 0.000
#&gt; GSM559412     4  0.3198      0.880 0.008 0.008 0.000 0.796 0.000 0.188
#&gt; GSM559413     4  0.3198      0.880 0.008 0.008 0.000 0.796 0.000 0.188
#&gt; GSM559415     1  0.0603      0.735 0.980 0.004 0.016 0.000 0.000 0.000
#&gt; GSM559416     1  0.0820      0.729 0.972 0.016 0.012 0.000 0.000 0.000
#&gt; GSM559417     1  0.0777      0.720 0.972 0.024 0.000 0.004 0.000 0.000
#&gt; GSM559418     1  0.0458      0.726 0.984 0.016 0.000 0.000 0.000 0.000
#&gt; GSM559419     1  0.0508      0.728 0.984 0.012 0.000 0.004 0.000 0.000
#&gt; GSM559420     6  0.0260      0.951 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; GSM559421     2  0.4402      0.760 0.184 0.712 0.000 0.000 0.104 0.000
#&gt; GSM559423     2  0.4465      0.753 0.260 0.684 0.000 0.004 0.048 0.004
#&gt; GSM559425     5  0.2706      0.856 0.000 0.104 0.000 0.036 0.860 0.000
#&gt; GSM559426     5  0.0725      0.917 0.000 0.012 0.000 0.012 0.976 0.000
#&gt; GSM559427     5  0.2706      0.856 0.000 0.104 0.000 0.036 0.860 0.000
#&gt; GSM559428     6  0.3449      0.778 0.000 0.116 0.000 0.076 0.000 0.808
#&gt; GSM559429     1  0.3693      0.493 0.800 0.148 0.004 0.020 0.028 0.000
#&gt; GSM559430     5  0.0000      0.923 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:kmeans 52          0.51516 2
#> ATC:kmeans 52          0.33864 3
#> ATC:kmeans 49          0.02175 4
#> ATC:kmeans 39          0.00349 5
#> ATC:kmeans 49          0.01469 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.969       0.987         0.5002 0.497   0.497
#> 3 3 1.000           0.993       0.996         0.3567 0.738   0.515
#> 4 4 0.898           0.901       0.926         0.0867 0.926   0.776
#> 5 5 0.857           0.894       0.909         0.0712 0.942   0.781
#> 6 6 0.846           0.798       0.853         0.0391 0.973   0.869
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1   0.000      0.971 1.000 0.000
#&gt; GSM559387     2   0.000      0.999 0.000 1.000
#&gt; GSM559391     2   0.000      0.999 0.000 1.000
#&gt; GSM559395     2   0.000      0.999 0.000 1.000
#&gt; GSM559397     1   0.000      0.971 1.000 0.000
#&gt; GSM559401     2   0.000      0.999 0.000 1.000
#&gt; GSM559414     2   0.184      0.970 0.028 0.972
#&gt; GSM559422     1   0.000      0.971 1.000 0.000
#&gt; GSM559424     2   0.000      0.999 0.000 1.000
#&gt; GSM559431     2   0.000      0.999 0.000 1.000
#&gt; GSM559432     2   0.000      0.999 0.000 1.000
#&gt; GSM559381     1   0.000      0.971 1.000 0.000
#&gt; GSM559382     1   0.000      0.971 1.000 0.000
#&gt; GSM559384     1   0.000      0.971 1.000 0.000
#&gt; GSM559385     2   0.000      0.999 0.000 1.000
#&gt; GSM559386     1   0.000      0.971 1.000 0.000
#&gt; GSM559388     2   0.000      0.999 0.000 1.000
#&gt; GSM559389     1   0.000      0.971 1.000 0.000
#&gt; GSM559390     1   0.000      0.971 1.000 0.000
#&gt; GSM559392     1   0.981      0.298 0.580 0.420
#&gt; GSM559393     2   0.000      0.999 0.000 1.000
#&gt; GSM559394     2   0.000      0.999 0.000 1.000
#&gt; GSM559396     1   0.000      0.971 1.000 0.000
#&gt; GSM559398     2   0.000      0.999 0.000 1.000
#&gt; GSM559399     2   0.000      0.999 0.000 1.000
#&gt; GSM559400     2   0.000      0.999 0.000 1.000
#&gt; GSM559402     1   0.000      0.971 1.000 0.000
#&gt; GSM559403     2   0.000      0.999 0.000 1.000
#&gt; GSM559404     1   0.000      0.971 1.000 0.000
#&gt; GSM559405     1   0.000      0.971 1.000 0.000
#&gt; GSM559406     1   0.000      0.971 1.000 0.000
#&gt; GSM559407     2   0.000      0.999 0.000 1.000
#&gt; GSM559408     1   0.000      0.971 1.000 0.000
#&gt; GSM559409     1   0.184      0.948 0.972 0.028
#&gt; GSM559410     2   0.000      0.999 0.000 1.000
#&gt; GSM559411     2   0.000      0.999 0.000 1.000
#&gt; GSM559412     1   0.000      0.971 1.000 0.000
#&gt; GSM559413     1   0.000      0.971 1.000 0.000
#&gt; GSM559415     2   0.000      0.999 0.000 1.000
#&gt; GSM559416     2   0.000      0.999 0.000 1.000
#&gt; GSM559417     2   0.000      0.999 0.000 1.000
#&gt; GSM559418     2   0.000      0.999 0.000 1.000
#&gt; GSM559419     1   0.671      0.783 0.824 0.176
#&gt; GSM559420     1   0.000      0.971 1.000 0.000
#&gt; GSM559421     2   0.000      0.999 0.000 1.000
#&gt; GSM559423     1   0.000      0.971 1.000 0.000
#&gt; GSM559425     2   0.000      0.999 0.000 1.000
#&gt; GSM559426     2   0.000      0.999 0.000 1.000
#&gt; GSM559427     2   0.000      0.999 0.000 1.000
#&gt; GSM559428     1   0.000      0.971 1.000 0.000
#&gt; GSM559429     2   0.000      0.999 0.000 1.000
#&gt; GSM559430     2   0.000      0.999 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559397     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559422     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559424     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559432     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559381     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559384     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559385     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559386     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559388     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559392     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559393     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559394     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559396     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559398     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559399     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559400     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559402     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559403     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559404     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559407     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559408     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559409     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559410     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559411     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM559412     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559415     3  0.0424      0.992 0.000 0.008 0.992
#&gt; GSM559416     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559417     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559418     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559419     1  0.5449      0.808 0.816 0.116 0.068
#&gt; GSM559420     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559423     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559425     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559428     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM559429     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559430     2  0.0000      1.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.1004      0.971 0.024 0.000 0.972 0.004
#&gt; GSM559387     3  0.0188      0.992 0.000 0.000 0.996 0.004
#&gt; GSM559391     3  0.0188      0.992 0.000 0.000 0.996 0.004
#&gt; GSM559395     3  0.0188      0.992 0.000 0.000 0.996 0.004
#&gt; GSM559397     1  0.3907      0.904 0.768 0.000 0.000 0.232
#&gt; GSM559401     3  0.0188      0.992 0.000 0.000 0.996 0.004
#&gt; GSM559414     3  0.0188      0.992 0.000 0.000 0.996 0.004
#&gt; GSM559422     1  0.3873      0.904 0.772 0.000 0.000 0.228
#&gt; GSM559424     3  0.0336      0.990 0.000 0.000 0.992 0.008
#&gt; GSM559431     2  0.1118      0.956 0.000 0.964 0.000 0.036
#&gt; GSM559432     2  0.1211      0.954 0.000 0.960 0.000 0.040
#&gt; GSM559381     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559382     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559384     1  0.3907      0.904 0.768 0.000 0.000 0.232
#&gt; GSM559385     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM559386     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559388     2  0.1211      0.954 0.000 0.960 0.000 0.040
#&gt; GSM559389     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559390     1  0.0188      0.846 0.996 0.000 0.000 0.004
#&gt; GSM559392     2  0.1118      0.950 0.000 0.964 0.000 0.036
#&gt; GSM559393     3  0.0817      0.972 0.000 0.000 0.976 0.024
#&gt; GSM559394     3  0.0336      0.988 0.000 0.000 0.992 0.008
#&gt; GSM559396     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559398     2  0.1118      0.950 0.000 0.964 0.000 0.036
#&gt; GSM559399     4  0.4500      0.545 0.000 0.000 0.316 0.684
#&gt; GSM559400     2  0.1118      0.950 0.000 0.964 0.000 0.036
#&gt; GSM559402     1  0.0188      0.843 0.996 0.000 0.004 0.000
#&gt; GSM559403     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM559404     1  0.0000      0.846 1.000 0.000 0.000 0.000
#&gt; GSM559405     1  0.3907      0.904 0.768 0.000 0.000 0.232
#&gt; GSM559406     1  0.0000      0.846 1.000 0.000 0.000 0.000
#&gt; GSM559407     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM559408     1  0.0000      0.846 1.000 0.000 0.000 0.000
#&gt; GSM559409     3  0.0707      0.976 0.020 0.000 0.980 0.000
#&gt; GSM559410     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM559411     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM559412     1  0.0000      0.846 1.000 0.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.846 1.000 0.000 0.000 0.000
#&gt; GSM559415     4  0.5021      0.652 0.000 0.036 0.240 0.724
#&gt; GSM559416     4  0.4250      0.679 0.000 0.276 0.000 0.724
#&gt; GSM559417     4  0.4522      0.616 0.000 0.320 0.000 0.680
#&gt; GSM559418     4  0.4331      0.668 0.000 0.288 0.000 0.712
#&gt; GSM559419     4  0.2922      0.624 0.104 0.008 0.004 0.884
#&gt; GSM559420     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559421     2  0.1118      0.950 0.000 0.964 0.000 0.036
#&gt; GSM559423     2  0.1302      0.944 0.000 0.956 0.000 0.044
#&gt; GSM559425     2  0.0000      0.958 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.1211      0.954 0.000 0.960 0.000 0.040
#&gt; GSM559427     2  0.0188      0.958 0.000 0.996 0.000 0.004
#&gt; GSM559428     1  0.3942      0.904 0.764 0.000 0.000 0.236
#&gt; GSM559429     2  0.1118      0.956 0.000 0.964 0.000 0.036
#&gt; GSM559430     2  0.1118      0.956 0.000 0.964 0.000 0.036
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     3  0.1341      0.919 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559387     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559391     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559395     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559397     1  0.0880      0.960 0.968 0.000 0.000 0.000 0.032
#&gt; GSM559401     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     1  0.1544      0.919 0.932 0.000 0.000 0.000 0.068
#&gt; GSM559424     3  0.0000      0.954 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559431     2  0.3513      0.815 0.000 0.800 0.000 0.180 0.020
#&gt; GSM559432     2  0.3513      0.815 0.000 0.800 0.000 0.180 0.020
#&gt; GSM559381     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559384     1  0.0162      0.985 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559385     3  0.1965      0.951 0.000 0.000 0.924 0.024 0.052
#&gt; GSM559386     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559388     2  0.3419      0.815 0.000 0.804 0.000 0.180 0.016
#&gt; GSM559389     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559390     5  0.3003      0.959 0.188 0.000 0.000 0.000 0.812
#&gt; GSM559392     2  0.2616      0.805 0.000 0.888 0.000 0.036 0.076
#&gt; GSM559393     3  0.2795      0.925 0.000 0.000 0.880 0.064 0.056
#&gt; GSM559394     3  0.2790      0.923 0.000 0.000 0.880 0.068 0.052
#&gt; GSM559396     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559398     2  0.2278      0.815 0.000 0.908 0.000 0.032 0.060
#&gt; GSM559399     4  0.4141      0.588 0.000 0.000 0.236 0.736 0.028
#&gt; GSM559400     2  0.2278      0.815 0.000 0.908 0.000 0.032 0.060
#&gt; GSM559402     5  0.2329      0.932 0.124 0.000 0.000 0.000 0.876
#&gt; GSM559403     3  0.1893      0.952 0.000 0.000 0.928 0.024 0.048
#&gt; GSM559404     5  0.2813      0.984 0.168 0.000 0.000 0.000 0.832
#&gt; GSM559405     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559406     5  0.2813      0.984 0.168 0.000 0.000 0.000 0.832
#&gt; GSM559407     3  0.1741      0.953 0.000 0.000 0.936 0.024 0.040
#&gt; GSM559408     5  0.2813      0.984 0.168 0.000 0.000 0.000 0.832
#&gt; GSM559409     3  0.2011      0.934 0.000 0.000 0.908 0.004 0.088
#&gt; GSM559410     3  0.1661      0.954 0.000 0.000 0.940 0.024 0.036
#&gt; GSM559411     3  0.1661      0.954 0.000 0.000 0.940 0.024 0.036
#&gt; GSM559412     5  0.2813      0.984 0.168 0.000 0.000 0.000 0.832
#&gt; GSM559413     5  0.2813      0.984 0.168 0.000 0.000 0.000 0.832
#&gt; GSM559415     4  0.1682      0.780 0.000 0.032 0.012 0.944 0.012
#&gt; GSM559416     4  0.1410      0.773 0.000 0.060 0.000 0.940 0.000
#&gt; GSM559417     4  0.4420      0.602 0.000 0.280 0.000 0.692 0.028
#&gt; GSM559418     4  0.2660      0.723 0.000 0.128 0.000 0.864 0.008
#&gt; GSM559419     4  0.4469      0.682 0.120 0.020 0.000 0.784 0.076
#&gt; GSM559420     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559421     2  0.2491      0.810 0.000 0.896 0.000 0.036 0.068
#&gt; GSM559423     2  0.3011      0.797 0.012 0.876 0.000 0.036 0.076
#&gt; GSM559425     2  0.0000      0.832 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.3513      0.815 0.000 0.800 0.000 0.180 0.020
#&gt; GSM559427     2  0.0162      0.832 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559428     1  0.0000      0.988 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559429     2  0.3513      0.815 0.000 0.800 0.000 0.180 0.020
#&gt; GSM559430     2  0.3513      0.815 0.000 0.800 0.000 0.180 0.020
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.1686      0.828 0.000 0.000 0.924 0.064 0.012 0.000
#&gt; GSM559387     3  0.0146      0.856 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559391     3  0.0146      0.856 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559395     3  0.0363      0.856 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; GSM559397     6  0.1116      0.950 0.008 0.000 0.000 0.028 0.004 0.960
#&gt; GSM559401     3  0.0146      0.856 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM559414     3  0.0405      0.856 0.000 0.000 0.988 0.004 0.008 0.000
#&gt; GSM559422     6  0.2957      0.815 0.008 0.000 0.000 0.140 0.016 0.836
#&gt; GSM559424     3  0.1285      0.847 0.052 0.000 0.944 0.000 0.004 0.000
#&gt; GSM559431     2  0.0000      0.770 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559432     2  0.0146      0.767 0.004 0.996 0.000 0.000 0.000 0.000
#&gt; GSM559381     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559382     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559384     6  0.0260      0.973 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; GSM559385     3  0.4493      0.806 0.084 0.000 0.736 0.020 0.160 0.000
#&gt; GSM559386     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559388     2  0.1219      0.736 0.004 0.948 0.000 0.000 0.048 0.000
#&gt; GSM559389     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559390     4  0.3420      0.859 0.040 0.000 0.000 0.840 0.060 0.060
#&gt; GSM559392     5  0.3782      0.915 0.000 0.412 0.000 0.000 0.588 0.000
#&gt; GSM559393     3  0.6009      0.601 0.192 0.000 0.536 0.020 0.252 0.000
#&gt; GSM559394     3  0.5543      0.623 0.248 0.000 0.572 0.004 0.176 0.000
#&gt; GSM559396     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559398     5  0.3854      0.873 0.000 0.464 0.000 0.000 0.536 0.000
#&gt; GSM559399     1  0.3644      0.620 0.792 0.000 0.120 0.000 0.088 0.000
#&gt; GSM559400     2  0.3868     -0.808 0.000 0.508 0.000 0.000 0.492 0.000
#&gt; GSM559402     4  0.0862      0.927 0.008 0.000 0.000 0.972 0.004 0.016
#&gt; GSM559403     3  0.4091      0.826 0.064 0.000 0.772 0.020 0.144 0.000
#&gt; GSM559404     4  0.1204      0.968 0.000 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559405     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559406     4  0.1204      0.968 0.000 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559407     3  0.3935      0.833 0.064 0.000 0.788 0.020 0.128 0.000
#&gt; GSM559408     4  0.1204      0.968 0.000 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559409     3  0.3784      0.810 0.008 0.000 0.792 0.124 0.076 0.000
#&gt; GSM559410     3  0.2870      0.852 0.040 0.000 0.856 0.004 0.100 0.000
#&gt; GSM559411     3  0.2963      0.853 0.036 0.000 0.856 0.012 0.096 0.000
#&gt; GSM559412     4  0.1204      0.968 0.000 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559413     4  0.1204      0.968 0.000 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559415     1  0.2604      0.737 0.880 0.076 0.008 0.000 0.036 0.000
#&gt; GSM559416     1  0.2762      0.724 0.804 0.196 0.000 0.000 0.000 0.000
#&gt; GSM559417     1  0.4954      0.503 0.552 0.052 0.000 0.008 0.388 0.000
#&gt; GSM559418     1  0.4924      0.674 0.668 0.212 0.000 0.008 0.112 0.000
#&gt; GSM559419     1  0.4546      0.695 0.724 0.000 0.000 0.032 0.192 0.052
#&gt; GSM559420     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559421     5  0.3828      0.912 0.000 0.440 0.000 0.000 0.560 0.000
#&gt; GSM559423     5  0.3955      0.878 0.000 0.384 0.000 0.000 0.608 0.008
#&gt; GSM559425     2  0.2823      0.463 0.000 0.796 0.000 0.000 0.204 0.000
#&gt; GSM559426     2  0.0146      0.767 0.004 0.996 0.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.2996      0.393 0.000 0.772 0.000 0.000 0.228 0.000
#&gt; GSM559428     6  0.0000      0.978 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559429     2  0.0000      0.770 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559430     2  0.0000      0.770 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> ATC:skmeans 51           0.3921 2
#> ATC:skmeans 52           0.0476 3
#> ATC:skmeans 52           0.0319 4
#> ATC:skmeans 52           0.0466 5
#> ATC:skmeans 49           0.0623 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.501           0.769       0.861         0.3287 0.735   0.735
#> 3 3 0.772           0.924       0.967         0.7559 0.667   0.551
#> 4 4 0.734           0.855       0.928         0.2663 0.791   0.524
#> 5 5 0.725           0.614       0.794         0.0791 0.864   0.551
#> 6 6 0.782           0.781       0.888         0.0447 0.834   0.396
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.0000      0.836 1.000 0.000
#&gt; GSM559387     1  0.0000      0.836 1.000 0.000
#&gt; GSM559391     1  0.0000      0.836 1.000 0.000
#&gt; GSM559395     1  0.0000      0.836 1.000 0.000
#&gt; GSM559397     1  0.8661      0.692 0.712 0.288
#&gt; GSM559401     1  0.0000      0.836 1.000 0.000
#&gt; GSM559414     1  0.0000      0.836 1.000 0.000
#&gt; GSM559422     1  0.8207      0.708 0.744 0.256
#&gt; GSM559424     1  0.0000      0.836 1.000 0.000
#&gt; GSM559431     2  0.8661      1.000 0.288 0.712
#&gt; GSM559432     2  0.8661      1.000 0.288 0.712
#&gt; GSM559381     1  0.8661      0.692 0.712 0.288
#&gt; GSM559382     1  0.8661      0.692 0.712 0.288
#&gt; GSM559384     1  0.8661      0.692 0.712 0.288
#&gt; GSM559385     1  0.0000      0.836 1.000 0.000
#&gt; GSM559386     1  0.8661      0.692 0.712 0.288
#&gt; GSM559388     1  0.9988     -0.510 0.520 0.480
#&gt; GSM559389     1  0.8661      0.692 0.712 0.288
#&gt; GSM559390     1  0.0000      0.836 1.000 0.000
#&gt; GSM559392     1  0.9988     -0.510 0.520 0.480
#&gt; GSM559393     1  0.0000      0.836 1.000 0.000
#&gt; GSM559394     1  0.0000      0.836 1.000 0.000
#&gt; GSM559396     1  0.0376      0.835 0.996 0.004
#&gt; GSM559398     2  0.8661      1.000 0.288 0.712
#&gt; GSM559399     1  0.0000      0.836 1.000 0.000
#&gt; GSM559400     1  0.4562      0.724 0.904 0.096
#&gt; GSM559402     1  0.0000      0.836 1.000 0.000
#&gt; GSM559403     1  0.0000      0.836 1.000 0.000
#&gt; GSM559404     1  0.8661      0.692 0.712 0.288
#&gt; GSM559405     1  0.8661      0.692 0.712 0.288
#&gt; GSM559406     1  0.0000      0.836 1.000 0.000
#&gt; GSM559407     1  0.0000      0.836 1.000 0.000
#&gt; GSM559408     1  0.0000      0.836 1.000 0.000
#&gt; GSM559409     1  0.0000      0.836 1.000 0.000
#&gt; GSM559410     1  0.0000      0.836 1.000 0.000
#&gt; GSM559411     1  0.0000      0.836 1.000 0.000
#&gt; GSM559412     1  0.8661      0.692 0.712 0.288
#&gt; GSM559413     1  0.8661      0.692 0.712 0.288
#&gt; GSM559415     1  0.0000      0.836 1.000 0.000
#&gt; GSM559416     1  0.0000      0.836 1.000 0.000
#&gt; GSM559417     1  0.0000      0.836 1.000 0.000
#&gt; GSM559418     1  0.0000      0.836 1.000 0.000
#&gt; GSM559419     1  0.0000      0.836 1.000 0.000
#&gt; GSM559420     1  0.8661      0.692 0.712 0.288
#&gt; GSM559421     2  0.8661      1.000 0.288 0.712
#&gt; GSM559423     1  0.4562      0.724 0.904 0.096
#&gt; GSM559425     2  0.8661      1.000 0.288 0.712
#&gt; GSM559426     2  0.8661      1.000 0.288 0.712
#&gt; GSM559427     2  0.8661      1.000 0.288 0.712
#&gt; GSM559428     1  0.8661      0.692 0.712 0.288
#&gt; GSM559429     1  0.2043      0.805 0.968 0.032
#&gt; GSM559430     2  0.8661      1.000 0.288 0.712
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559387     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559391     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559395     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559397     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559401     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559414     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559422     3   0.388      0.825 0.152 0.000 0.848
#&gt; GSM559424     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559431     2   0.000      0.893 0.000 1.000 0.000
#&gt; GSM559432     2   0.288      0.834 0.096 0.904 0.000
#&gt; GSM559381     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559382     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559384     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559385     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559386     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559388     1   0.327      0.866 0.884 0.116 0.000
#&gt; GSM559389     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559390     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559392     2   0.604      0.408 0.380 0.620 0.000
#&gt; GSM559393     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559394     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559396     1   0.341      0.854 0.876 0.000 0.124
#&gt; GSM559398     2   0.000      0.893 0.000 1.000 0.000
#&gt; GSM559399     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559400     1   0.327      0.866 0.884 0.116 0.000
#&gt; GSM559402     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559403     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559404     3   0.327      0.870 0.116 0.000 0.884
#&gt; GSM559405     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559406     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559407     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559408     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559409     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559410     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559411     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559412     3   0.327      0.870 0.116 0.000 0.884
#&gt; GSM559413     3   0.327      0.870 0.116 0.000 0.884
#&gt; GSM559415     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559416     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559417     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559418     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559419     1   0.000      0.977 1.000 0.000 0.000
#&gt; GSM559420     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559421     2   0.369      0.794 0.140 0.860 0.000
#&gt; GSM559423     1   0.327      0.866 0.884 0.116 0.000
#&gt; GSM559425     2   0.000      0.893 0.000 1.000 0.000
#&gt; GSM559426     2   0.000      0.893 0.000 1.000 0.000
#&gt; GSM559427     2   0.000      0.893 0.000 1.000 0.000
#&gt; GSM559428     3   0.000      0.942 0.000 0.000 1.000
#&gt; GSM559429     1   0.327      0.866 0.884 0.116 0.000
#&gt; GSM559430     2   0.000      0.893 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     3  0.1389      0.862 0.000 0.000 0.952 0.048
#&gt; GSM559387     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559391     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559395     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559397     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559401     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559422     4  0.4088      0.744 0.140 0.000 0.040 0.820
#&gt; GSM559424     3  0.3528      0.855 0.000 0.000 0.808 0.192
#&gt; GSM559431     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM559432     2  0.3569      0.778 0.000 0.804 0.196 0.000
#&gt; GSM559381     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559384     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559385     3  0.3528      0.855 0.000 0.000 0.808 0.192
#&gt; GSM559386     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559388     4  0.4193      0.613 0.000 0.268 0.000 0.732
#&gt; GSM559389     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559392     4  0.1211      0.904 0.000 0.040 0.000 0.960
#&gt; GSM559393     4  0.2921      0.808 0.000 0.000 0.140 0.860
#&gt; GSM559394     3  0.4103      0.782 0.000 0.000 0.744 0.256
#&gt; GSM559396     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559398     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM559399     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559400     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559402     4  0.2921      0.808 0.000 0.000 0.140 0.860
#&gt; GSM559403     3  0.3569      0.852 0.000 0.000 0.804 0.196
#&gt; GSM559404     1  0.3105      0.790 0.856 0.000 0.140 0.004
#&gt; GSM559405     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559406     4  0.0707      0.922 0.000 0.000 0.020 0.980
#&gt; GSM559407     3  0.3528      0.855 0.000 0.000 0.808 0.192
#&gt; GSM559408     4  0.2921      0.808 0.000 0.000 0.140 0.860
#&gt; GSM559409     3  0.3942      0.812 0.000 0.000 0.764 0.236
#&gt; GSM559410     3  0.0000      0.854 0.000 0.000 1.000 0.000
#&gt; GSM559411     3  0.3219      0.863 0.000 0.000 0.836 0.164
#&gt; GSM559412     1  0.7220      0.011 0.440 0.000 0.140 0.420
#&gt; GSM559413     1  0.3377      0.784 0.848 0.000 0.140 0.012
#&gt; GSM559415     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559416     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559417     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559418     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559419     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559420     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559421     2  0.4193      0.608 0.000 0.732 0.000 0.268
#&gt; GSM559423     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559425     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000      0.927 0.000 1.000 0.000 0.000
#&gt; GSM559428     1  0.0000      0.908 1.000 0.000 0.000 0.000
#&gt; GSM559429     4  0.0000      0.934 0.000 0.000 0.000 1.000
#&gt; GSM559430     2  0.0000      0.927 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     1  0.2886    -0.1367 0.844 0.000 0.148 0.008 0.000
#&gt; GSM559387     3  0.4268     1.0000 0.444 0.000 0.556 0.000 0.000
#&gt; GSM559391     1  0.4114    -0.6216 0.624 0.000 0.376 0.000 0.000
#&gt; GSM559395     3  0.4268     1.0000 0.444 0.000 0.556 0.000 0.000
#&gt; GSM559397     5  0.0703     0.9741 0.024 0.000 0.000 0.000 0.976
#&gt; GSM559401     3  0.4268     1.0000 0.444 0.000 0.556 0.000 0.000
#&gt; GSM559414     3  0.4268     1.0000 0.444 0.000 0.556 0.000 0.000
#&gt; GSM559422     4  0.5884     0.2740 0.352 0.000 0.112 0.536 0.000
#&gt; GSM559424     1  0.5104     0.1325 0.648 0.000 0.068 0.284 0.000
#&gt; GSM559431     2  0.0000     0.8740 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     2  0.4907     0.1664 0.000 0.488 0.488 0.024 0.000
#&gt; GSM559381     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559382     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559384     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559385     1  0.4418     0.2130 0.652 0.000 0.016 0.332 0.000
#&gt; GSM559386     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559388     4  0.4430     0.6375 0.000 0.172 0.076 0.752 0.000
#&gt; GSM559389     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559390     4  0.1741     0.7812 0.024 0.000 0.040 0.936 0.000
#&gt; GSM559392     4  0.4732     0.5818 0.000 0.208 0.076 0.716 0.000
#&gt; GSM559393     4  0.3336     0.5207 0.228 0.000 0.000 0.772 0.000
#&gt; GSM559394     1  0.4418     0.2130 0.652 0.000 0.016 0.332 0.000
#&gt; GSM559396     4  0.5845     0.2803 0.352 0.000 0.108 0.540 0.000
#&gt; GSM559398     2  0.1671     0.8381 0.000 0.924 0.076 0.000 0.000
#&gt; GSM559399     4  0.0880     0.7998 0.032 0.000 0.000 0.968 0.000
#&gt; GSM559400     4  0.1671     0.7792 0.000 0.000 0.076 0.924 0.000
#&gt; GSM559402     1  0.5689    -0.0598 0.480 0.000 0.080 0.440 0.000
#&gt; GSM559403     1  0.4138     0.2737 0.616 0.000 0.000 0.384 0.000
#&gt; GSM559404     1  0.5825     0.4127 0.556 0.000 0.368 0.052 0.024
#&gt; GSM559405     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559406     1  0.6695     0.1802 0.392 0.000 0.368 0.240 0.000
#&gt; GSM559407     1  0.4329     0.2169 0.672 0.000 0.016 0.312 0.000
#&gt; GSM559408     1  0.5530     0.4052 0.556 0.000 0.368 0.076 0.000
#&gt; GSM559409     1  0.2852     0.3109 0.828 0.000 0.000 0.172 0.000
#&gt; GSM559410     3  0.4268     1.0000 0.444 0.000 0.556 0.000 0.000
#&gt; GSM559411     1  0.4435    -0.5321 0.648 0.000 0.336 0.016 0.000
#&gt; GSM559412     1  0.5774     0.4123 0.556 0.000 0.368 0.060 0.016
#&gt; GSM559413     1  0.5774     0.4123 0.556 0.000 0.368 0.060 0.016
#&gt; GSM559415     4  0.0880     0.7998 0.032 0.000 0.000 0.968 0.000
#&gt; GSM559416     4  0.1270     0.7766 0.052 0.000 0.000 0.948 0.000
#&gt; GSM559417     4  0.0000     0.8004 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559418     4  0.0880     0.7998 0.032 0.000 0.000 0.968 0.000
#&gt; GSM559419     4  0.0880     0.7998 0.032 0.000 0.000 0.968 0.000
#&gt; GSM559420     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559421     2  0.4933     0.5823 0.000 0.688 0.076 0.236 0.000
#&gt; GSM559423     4  0.1671     0.7792 0.000 0.000 0.076 0.924 0.000
#&gt; GSM559425     2  0.0000     0.8740 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0000     0.8740 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559427     2  0.0000     0.8740 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     5  0.0000     0.9968 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559429     4  0.1270     0.7887 0.000 0.000 0.052 0.948 0.000
#&gt; GSM559430     2  0.0000     0.8740 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     3  0.2805     0.7858 0.000 0.004 0.812 0.184 0.000 0.000
#&gt; GSM559387     3  0.0000     0.8852 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559391     3  0.0937     0.8739 0.000 0.000 0.960 0.040 0.000 0.000
#&gt; GSM559395     3  0.0000     0.8852 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559397     6  0.1075     0.9436 0.000 0.000 0.000 0.048 0.000 0.952
#&gt; GSM559401     3  0.0000     0.8852 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559414     3  0.0000     0.8852 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559422     4  0.2762     0.7617 0.196 0.000 0.000 0.804 0.000 0.000
#&gt; GSM559424     3  0.4919     0.5988 0.204 0.004 0.664 0.128 0.000 0.000
#&gt; GSM559431     5  0.0000     0.8040 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559432     5  0.4035     0.5794 0.004 0.196 0.056 0.000 0.744 0.000
#&gt; GSM559381     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559382     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559384     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559385     1  0.4264     0.7180 0.744 0.004 0.124 0.128 0.000 0.000
#&gt; GSM559386     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559388     1  0.3547     0.4222 0.668 0.332 0.000 0.000 0.000 0.000
#&gt; GSM559389     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559390     1  0.1007     0.8223 0.956 0.000 0.000 0.044 0.000 0.000
#&gt; GSM559392     2  0.1349     0.6570 0.056 0.940 0.000 0.004 0.000 0.000
#&gt; GSM559393     1  0.2278     0.7935 0.868 0.004 0.000 0.128 0.000 0.000
#&gt; GSM559394     1  0.3608     0.7624 0.800 0.004 0.068 0.128 0.000 0.000
#&gt; GSM559396     4  0.2793     0.7592 0.200 0.000 0.000 0.800 0.000 0.000
#&gt; GSM559398     2  0.3175     0.3605 0.000 0.744 0.000 0.000 0.256 0.000
#&gt; GSM559399     1  0.0000     0.8473 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559400     2  0.3868     0.0164 0.496 0.504 0.000 0.000 0.000 0.000
#&gt; GSM559402     4  0.2631     0.7463 0.180 0.000 0.000 0.820 0.000 0.000
#&gt; GSM559403     1  0.3663     0.7605 0.796 0.004 0.072 0.128 0.000 0.000
#&gt; GSM559404     4  0.0146     0.8419 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559405     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559406     4  0.2003     0.7924 0.116 0.000 0.000 0.884 0.000 0.000
#&gt; GSM559407     1  0.5044     0.5856 0.644 0.004 0.224 0.128 0.000 0.000
#&gt; GSM559408     4  0.0000     0.8417 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559409     4  0.4509     0.5989 0.104 0.004 0.180 0.712 0.000 0.000
#&gt; GSM559410     3  0.0000     0.8852 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559411     3  0.3716     0.7696 0.076 0.004 0.792 0.128 0.000 0.000
#&gt; GSM559412     4  0.0146     0.8419 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559413     4  0.0146     0.8419 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559415     1  0.0000     0.8473 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559416     1  0.0000     0.8473 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559417     1  0.0146     0.8465 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559418     1  0.0000     0.8473 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559419     1  0.0146     0.8465 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559420     6  0.0000     0.9877 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559421     2  0.1267     0.6133 0.000 0.940 0.000 0.000 0.060 0.000
#&gt; GSM559423     2  0.2320     0.6387 0.132 0.864 0.000 0.004 0.000 0.000
#&gt; GSM559425     5  0.3244     0.6554 0.000 0.268 0.000 0.000 0.732 0.000
#&gt; GSM559426     5  0.0000     0.8040 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559427     5  0.3428     0.6119 0.000 0.304 0.000 0.000 0.696 0.000
#&gt; GSM559428     6  0.1141     0.9539 0.000 0.052 0.000 0.000 0.000 0.948
#&gt; GSM559429     1  0.0146     0.8457 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM559430     5  0.0000     0.8040 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:pam 50         1.000000 2
#> ATC:pam 51         0.816772 3
#> ATC:pam 51         0.014854 4
#> ATC:pam 34         0.000904 5
#> ATC:pam 49         0.000481 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.451           0.781       0.770         0.3695 0.517   0.517
#> 3 3 0.509           0.822       0.850         0.7038 0.756   0.556
#> 4 4 0.765           0.789       0.900         0.1436 0.888   0.682
#> 5 5 0.697           0.788       0.856         0.0478 0.920   0.709
#> 6 6 0.749           0.710       0.813         0.0653 0.925   0.685
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     2   0.998     -0.831 0.476 0.524
#&gt; GSM559387     2   0.833      0.594 0.264 0.736
#&gt; GSM559391     1   0.991      0.998 0.556 0.444
#&gt; GSM559395     2   0.738      0.633 0.208 0.792
#&gt; GSM559397     2   0.991      0.481 0.444 0.556
#&gt; GSM559401     2   0.991      0.481 0.444 0.556
#&gt; GSM559414     2   0.991      0.481 0.444 0.556
#&gt; GSM559422     2   0.991      0.481 0.444 0.556
#&gt; GSM559424     1   0.991      0.998 0.556 0.444
#&gt; GSM559431     2   0.184      0.774 0.028 0.972
#&gt; GSM559432     2   0.991      0.481 0.444 0.556
#&gt; GSM559381     2   0.000      0.792 0.000 1.000
#&gt; GSM559382     2   0.000      0.792 0.000 1.000
#&gt; GSM559384     2   0.000      0.792 0.000 1.000
#&gt; GSM559385     1   0.991      0.998 0.556 0.444
#&gt; GSM559386     2   0.000      0.792 0.000 1.000
#&gt; GSM559388     2   0.000      0.792 0.000 1.000
#&gt; GSM559389     2   0.000      0.792 0.000 1.000
#&gt; GSM559390     1   0.991      0.998 0.556 0.444
#&gt; GSM559392     2   0.000      0.792 0.000 1.000
#&gt; GSM559393     1   0.991      0.998 0.556 0.444
#&gt; GSM559394     1   0.992      0.992 0.552 0.448
#&gt; GSM559396     2   0.861     -0.175 0.284 0.716
#&gt; GSM559398     2   0.000      0.792 0.000 1.000
#&gt; GSM559399     1   0.991      0.998 0.556 0.444
#&gt; GSM559400     2   0.000      0.792 0.000 1.000
#&gt; GSM559402     1   0.991      0.998 0.556 0.444
#&gt; GSM559403     1   0.991      0.998 0.556 0.444
#&gt; GSM559404     2   0.000      0.792 0.000 1.000
#&gt; GSM559405     2   0.000      0.792 0.000 1.000
#&gt; GSM559406     1   0.991      0.998 0.556 0.444
#&gt; GSM559407     1   0.991      0.998 0.556 0.444
#&gt; GSM559408     1   0.991      0.998 0.556 0.444
#&gt; GSM559409     1   0.991      0.998 0.556 0.444
#&gt; GSM559410     2   0.000      0.792 0.000 1.000
#&gt; GSM559411     1   0.995      0.971 0.540 0.460
#&gt; GSM559412     1   0.991      0.998 0.556 0.444
#&gt; GSM559413     2   0.373      0.664 0.072 0.928
#&gt; GSM559415     1   0.991      0.998 0.556 0.444
#&gt; GSM559416     1   0.991      0.998 0.556 0.444
#&gt; GSM559417     1   0.991      0.998 0.556 0.444
#&gt; GSM559418     1   0.991      0.998 0.556 0.444
#&gt; GSM559419     1   0.991      0.998 0.556 0.444
#&gt; GSM559420     2   0.000      0.792 0.000 1.000
#&gt; GSM559421     2   0.000      0.792 0.000 1.000
#&gt; GSM559423     2   0.000      0.792 0.000 1.000
#&gt; GSM559425     2   0.000      0.792 0.000 1.000
#&gt; GSM559426     2   0.000      0.792 0.000 1.000
#&gt; GSM559427     2   0.000      0.792 0.000 1.000
#&gt; GSM559428     2   0.000      0.792 0.000 1.000
#&gt; GSM559429     2   0.163      0.776 0.024 0.976
#&gt; GSM559430     2   0.000      0.792 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     1  0.6359     0.0677 0.592 0.404 0.004
#&gt; GSM559387     2  0.8109     0.7907 0.116 0.628 0.256
#&gt; GSM559391     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559395     2  0.8331     0.7739 0.164 0.628 0.208
#&gt; GSM559397     2  0.6204     0.6752 0.000 0.576 0.424
#&gt; GSM559401     2  0.8109     0.7907 0.116 0.628 0.256
#&gt; GSM559414     2  0.8109     0.7907 0.116 0.628 0.256
#&gt; GSM559422     2  0.6008     0.7322 0.000 0.628 0.372
#&gt; GSM559424     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559431     2  0.3619     0.7896 0.000 0.864 0.136
#&gt; GSM559432     2  0.5178     0.7817 0.000 0.744 0.256
#&gt; GSM559381     3  0.2537     0.8366 0.080 0.000 0.920
#&gt; GSM559382     2  0.7513     0.6593 0.052 0.604 0.344
#&gt; GSM559384     3  0.4796     0.8397 0.220 0.000 0.780
#&gt; GSM559385     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559386     3  0.2537     0.8366 0.080 0.000 0.920
#&gt; GSM559388     2  0.3134     0.7831 0.032 0.916 0.052
#&gt; GSM559389     3  0.2537     0.8366 0.080 0.000 0.920
#&gt; GSM559390     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559392     2  0.4270     0.7829 0.116 0.860 0.024
#&gt; GSM559393     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559394     1  0.0424     0.9535 0.992 0.008 0.000
#&gt; GSM559396     3  0.5497     0.7910 0.292 0.000 0.708
#&gt; GSM559398     2  0.0000     0.7632 0.000 1.000 0.000
#&gt; GSM559399     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559400     2  0.5988     0.7772 0.168 0.776 0.056
#&gt; GSM559402     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559403     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559404     3  0.5260     0.7516 0.080 0.092 0.828
#&gt; GSM559405     3  0.2537     0.8366 0.080 0.000 0.920
#&gt; GSM559406     3  0.5760     0.7409 0.328 0.000 0.672
#&gt; GSM559407     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559408     3  0.5882     0.7049 0.348 0.000 0.652
#&gt; GSM559409     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559410     2  0.8350     0.7485 0.196 0.628 0.176
#&gt; GSM559411     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559412     3  0.5497     0.7910 0.292 0.000 0.708
#&gt; GSM559413     3  0.4887     0.8367 0.228 0.000 0.772
#&gt; GSM559415     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559416     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559417     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559418     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559419     1  0.0000     0.9636 1.000 0.000 0.000
#&gt; GSM559420     3  0.2682     0.8325 0.076 0.004 0.920
#&gt; GSM559421     2  0.3267     0.7738 0.116 0.884 0.000
#&gt; GSM559423     2  0.8028     0.7747 0.168 0.656 0.176
#&gt; GSM559425     2  0.0000     0.7632 0.000 1.000 0.000
#&gt; GSM559426     2  0.0592     0.7644 0.012 0.988 0.000
#&gt; GSM559427     2  0.0000     0.7632 0.000 1.000 0.000
#&gt; GSM559428     2  0.7513     0.6593 0.052 0.604 0.344
#&gt; GSM559429     2  0.8028     0.7747 0.168 0.656 0.176
#&gt; GSM559430     2  0.0000     0.7632 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     4  0.3649      0.689 0.000 0.000 0.204 0.796
#&gt; GSM559387     3  0.1716      0.756 0.000 0.000 0.936 0.064
#&gt; GSM559391     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559395     3  0.7812      0.167 0.000 0.256 0.396 0.348
#&gt; GSM559397     3  0.1022      0.781 0.032 0.000 0.968 0.000
#&gt; GSM559401     3  0.0000      0.785 0.000 0.000 1.000 0.000
#&gt; GSM559414     3  0.0000      0.785 0.000 0.000 1.000 0.000
#&gt; GSM559422     3  0.0000      0.785 0.000 0.000 1.000 0.000
#&gt; GSM559424     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559431     2  0.1557      0.851 0.000 0.944 0.056 0.000
#&gt; GSM559432     3  0.0000      0.785 0.000 0.000 1.000 0.000
#&gt; GSM559381     1  0.0000      0.751 1.000 0.000 0.000 0.000
#&gt; GSM559382     3  0.4992      0.393 0.476 0.000 0.524 0.000
#&gt; GSM559384     1  0.3907      0.824 0.768 0.000 0.000 0.232
#&gt; GSM559385     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559386     1  0.0000      0.751 1.000 0.000 0.000 0.000
#&gt; GSM559388     2  0.3942      0.574 0.000 0.764 0.000 0.236
#&gt; GSM559389     1  0.0000      0.751 1.000 0.000 0.000 0.000
#&gt; GSM559390     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559392     2  0.0000      0.894 0.000 1.000 0.000 0.000
#&gt; GSM559393     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559394     4  0.0592      0.932 0.000 0.016 0.000 0.984
#&gt; GSM559396     1  0.4008      0.819 0.756 0.000 0.000 0.244
#&gt; GSM559398     2  0.0000      0.894 0.000 1.000 0.000 0.000
#&gt; GSM559399     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559400     2  0.1302      0.860 0.000 0.956 0.000 0.044
#&gt; GSM559402     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559403     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559404     1  0.3942      0.824 0.764 0.000 0.000 0.236
#&gt; GSM559405     1  0.0000      0.751 1.000 0.000 0.000 0.000
#&gt; GSM559406     1  0.4134      0.803 0.740 0.000 0.000 0.260
#&gt; GSM559407     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559408     1  0.4222      0.791 0.728 0.000 0.000 0.272
#&gt; GSM559409     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559410     4  0.7824     -0.187 0.000 0.264 0.336 0.400
#&gt; GSM559411     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559412     1  0.3942      0.824 0.764 0.000 0.000 0.236
#&gt; GSM559413     1  0.3942      0.824 0.764 0.000 0.000 0.236
#&gt; GSM559415     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559416     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559417     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559418     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559419     4  0.0000      0.948 0.000 0.000 0.000 1.000
#&gt; GSM559420     1  0.0000      0.751 1.000 0.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.894 0.000 1.000 0.000 0.000
#&gt; GSM559423     3  0.5592      0.477 0.000 0.300 0.656 0.044
#&gt; GSM559425     2  0.0000      0.894 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0336      0.890 0.000 0.992 0.000 0.008
#&gt; GSM559427     2  0.0000      0.894 0.000 1.000 0.000 0.000
#&gt; GSM559428     3  0.4713      0.581 0.360 0.000 0.640 0.000
#&gt; GSM559429     2  0.6738      0.148 0.000 0.544 0.352 0.104
#&gt; GSM559430     2  0.0000      0.894 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     1  0.4047      0.404 0.676 0.000 0.320 0.004 0.000
#&gt; GSM559387     3  0.1478      0.737 0.064 0.000 0.936 0.000 0.000
#&gt; GSM559391     1  0.0324      0.940 0.992 0.000 0.004 0.004 0.000
#&gt; GSM559395     3  0.3219      0.702 0.136 0.020 0.840 0.004 0.000
#&gt; GSM559397     3  0.5198      0.660 0.000 0.000 0.688 0.164 0.148
#&gt; GSM559401     3  0.0000      0.731 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559414     3  0.0000      0.731 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559422     3  0.3242      0.709 0.000 0.000 0.784 0.216 0.000
#&gt; GSM559424     1  0.0162      0.941 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559431     2  0.1732      0.834 0.000 0.920 0.080 0.000 0.000
#&gt; GSM559432     3  0.3242      0.709 0.000 0.000 0.784 0.216 0.000
#&gt; GSM559381     5  0.2280      0.837 0.000 0.000 0.000 0.120 0.880
#&gt; GSM559382     5  0.3885      0.535 0.000 0.000 0.268 0.008 0.724
#&gt; GSM559384     4  0.5079      0.707 0.136 0.000 0.000 0.700 0.164
#&gt; GSM559385     1  0.0162      0.941 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559386     5  0.2280      0.837 0.000 0.000 0.000 0.120 0.880
#&gt; GSM559388     2  0.4555      0.595 0.224 0.720 0.000 0.056 0.000
#&gt; GSM559389     5  0.2280      0.837 0.000 0.000 0.000 0.120 0.880
#&gt; GSM559390     1  0.1364      0.932 0.952 0.000 0.000 0.036 0.012
#&gt; GSM559392     2  0.0510      0.899 0.016 0.984 0.000 0.000 0.000
#&gt; GSM559393     1  0.0566      0.941 0.984 0.000 0.000 0.004 0.012
#&gt; GSM559394     1  0.0451      0.939 0.988 0.000 0.008 0.004 0.000
#&gt; GSM559396     4  0.4645      0.766 0.204 0.000 0.000 0.724 0.072
#&gt; GSM559398     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.1124      0.935 0.960 0.000 0.000 0.036 0.004
#&gt; GSM559400     2  0.2179      0.814 0.112 0.888 0.000 0.000 0.000
#&gt; GSM559402     1  0.0162      0.941 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559403     1  0.0162      0.941 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559404     4  0.7397      0.168 0.056 0.000 0.248 0.480 0.216
#&gt; GSM559405     5  0.2329      0.834 0.000 0.000 0.000 0.124 0.876
#&gt; GSM559406     4  0.4840      0.729 0.320 0.000 0.000 0.640 0.040
#&gt; GSM559407     1  0.0162      0.942 0.996 0.000 0.004 0.000 0.000
#&gt; GSM559408     4  0.4836      0.681 0.356 0.000 0.000 0.612 0.032
#&gt; GSM559409     1  0.0162      0.941 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559410     3  0.4706      0.470 0.344 0.020 0.632 0.004 0.000
#&gt; GSM559411     1  0.1124      0.916 0.960 0.000 0.036 0.004 0.000
#&gt; GSM559412     4  0.4605      0.773 0.192 0.000 0.000 0.732 0.076
#&gt; GSM559413     4  0.4698      0.761 0.172 0.000 0.000 0.732 0.096
#&gt; GSM559415     1  0.0880      0.937 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559416     1  0.0963      0.936 0.964 0.000 0.000 0.036 0.000
#&gt; GSM559417     1  0.1918      0.918 0.928 0.000 0.000 0.036 0.036
#&gt; GSM559418     1  0.1836      0.921 0.932 0.000 0.000 0.036 0.032
#&gt; GSM559419     1  0.1918      0.918 0.928 0.000 0.000 0.036 0.036
#&gt; GSM559420     5  0.2280      0.837 0.000 0.000 0.000 0.120 0.880
#&gt; GSM559421     2  0.0510      0.899 0.016 0.984 0.000 0.000 0.000
#&gt; GSM559423     3  0.7026      0.614 0.112 0.224 0.580 0.008 0.076
#&gt; GSM559425     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.2488      0.788 0.124 0.872 0.000 0.004 0.000
#&gt; GSM559427     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     5  0.3885      0.535 0.000 0.000 0.268 0.008 0.724
#&gt; GSM559429     3  0.6506      0.377 0.208 0.324 0.468 0.000 0.000
#&gt; GSM559430     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559383     4  0.6245     -0.305 0.284 0.000 0.008 0.420 0.288 0.000
#&gt; GSM559387     3  0.4372      0.805 0.028 0.000 0.692 0.020 0.260 0.000
#&gt; GSM559391     1  0.3499      0.425 0.680 0.000 0.000 0.000 0.320 0.000
#&gt; GSM559395     3  0.6153      0.616 0.088 0.008 0.540 0.052 0.312 0.000
#&gt; GSM559397     3  0.3133      0.740 0.000 0.000 0.780 0.212 0.000 0.008
#&gt; GSM559401     3  0.2945      0.840 0.000 0.000 0.824 0.020 0.156 0.000
#&gt; GSM559414     3  0.2945      0.840 0.000 0.000 0.824 0.020 0.156 0.000
#&gt; GSM559422     3  0.0363      0.827 0.000 0.000 0.988 0.012 0.000 0.000
#&gt; GSM559424     1  0.3876      0.454 0.700 0.000 0.000 0.024 0.276 0.000
#&gt; GSM559431     2  0.1180      0.872 0.004 0.960 0.008 0.024 0.000 0.004
#&gt; GSM559432     3  0.0000      0.829 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559381     6  0.0000      0.890 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559382     6  0.3323      0.778 0.000 0.000 0.008 0.204 0.008 0.780
#&gt; GSM559384     4  0.4230      0.615 0.024 0.000 0.000 0.612 0.000 0.364
#&gt; GSM559385     1  0.3428      0.452 0.696 0.000 0.000 0.000 0.304 0.000
#&gt; GSM559386     6  0.0146      0.892 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM559388     2  0.3933      0.646 0.216 0.740 0.040 0.004 0.000 0.000
#&gt; GSM559389     6  0.0146      0.892 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM559390     1  0.0603      0.722 0.980 0.000 0.000 0.004 0.016 0.000
#&gt; GSM559392     2  0.0508      0.879 0.004 0.984 0.000 0.000 0.012 0.000
#&gt; GSM559393     1  0.1341      0.712 0.948 0.000 0.000 0.024 0.028 0.000
#&gt; GSM559394     1  0.5137      0.294 0.604 0.004 0.000 0.104 0.288 0.000
#&gt; GSM559396     4  0.6059      0.725 0.084 0.000 0.088 0.600 0.004 0.224
#&gt; GSM559398     2  0.0000      0.879 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559399     1  0.0547      0.726 0.980 0.000 0.000 0.000 0.020 0.000
#&gt; GSM559400     2  0.3441      0.770 0.140 0.816 0.000 0.012 0.028 0.004
#&gt; GSM559402     1  0.0508      0.728 0.984 0.000 0.000 0.004 0.012 0.000
#&gt; GSM559403     1  0.3371      0.470 0.708 0.000 0.000 0.000 0.292 0.000
#&gt; GSM559404     4  0.4925      0.733 0.048 0.000 0.032 0.692 0.008 0.220
#&gt; GSM559405     6  0.1471      0.851 0.004 0.000 0.064 0.000 0.000 0.932
#&gt; GSM559406     4  0.4516      0.769 0.112 0.000 0.000 0.700 0.000 0.188
#&gt; GSM559407     5  0.3765      0.708 0.404 0.000 0.000 0.000 0.596 0.000
#&gt; GSM559408     4  0.4233      0.553 0.268 0.000 0.000 0.684 0.000 0.048
#&gt; GSM559409     1  0.4371      0.400 0.664 0.000 0.000 0.052 0.284 0.000
#&gt; GSM559410     5  0.4572      0.707 0.240 0.008 0.032 0.020 0.700 0.000
#&gt; GSM559411     5  0.4105      0.784 0.348 0.000 0.000 0.020 0.632 0.000
#&gt; GSM559412     4  0.4349      0.770 0.084 0.000 0.000 0.708 0.000 0.208
#&gt; GSM559413     4  0.4255      0.762 0.068 0.000 0.000 0.708 0.000 0.224
#&gt; GSM559415     1  0.0935      0.727 0.964 0.000 0.000 0.004 0.032 0.000
#&gt; GSM559416     1  0.0692      0.728 0.976 0.000 0.000 0.000 0.020 0.004
#&gt; GSM559417     1  0.1219      0.699 0.948 0.000 0.000 0.000 0.048 0.004
#&gt; GSM559418     1  0.1152      0.707 0.952 0.000 0.000 0.004 0.044 0.000
#&gt; GSM559419     1  0.1082      0.706 0.956 0.000 0.000 0.004 0.040 0.000
#&gt; GSM559420     6  0.0146      0.892 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM559421     2  0.0508      0.879 0.004 0.984 0.000 0.000 0.012 0.000
#&gt; GSM559423     2  0.6084      0.622 0.124 0.616 0.012 0.208 0.028 0.012
#&gt; GSM559425     2  0.0000      0.879 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.1152      0.865 0.044 0.952 0.000 0.004 0.000 0.000
#&gt; GSM559427     2  0.0000      0.879 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559428     6  0.3381      0.772 0.000 0.000 0.008 0.212 0.008 0.772
#&gt; GSM559429     2  0.5827      0.645 0.172 0.660 0.032 0.044 0.092 0.000
#&gt; GSM559430     2  0.0146      0.880 0.004 0.996 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:mclust 45         1.00e+00 2
#> ATC:mclust 51         2.34e-02 3
#> ATC:mclust 47         7.96e-05 4
#> ATC:mclust 48         2.46e-05 5
#> ATC:mclust 45         2.69e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21167 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.958       0.982         0.4051 0.599   0.599
#> 3 3 0.922           0.925       0.968         0.6577 0.660   0.463
#> 4 4 0.619           0.606       0.789         0.0956 0.900   0.715
#> 5 5 0.672           0.699       0.828         0.0743 0.881   0.595
#> 6 6 0.629           0.511       0.711         0.0380 0.956   0.801
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559383     1  0.0000      0.983 1.000 0.000
#&gt; GSM559387     1  0.0000      0.983 1.000 0.000
#&gt; GSM559391     1  0.0000      0.983 1.000 0.000
#&gt; GSM559395     1  0.0000      0.983 1.000 0.000
#&gt; GSM559397     1  0.0000      0.983 1.000 0.000
#&gt; GSM559401     1  0.0000      0.983 1.000 0.000
#&gt; GSM559414     1  0.0000      0.983 1.000 0.000
#&gt; GSM559422     1  0.0000      0.983 1.000 0.000
#&gt; GSM559424     1  0.0000      0.983 1.000 0.000
#&gt; GSM559431     2  0.0000      0.973 0.000 1.000
#&gt; GSM559432     2  0.0000      0.973 0.000 1.000
#&gt; GSM559381     1  0.0000      0.983 1.000 0.000
#&gt; GSM559382     1  0.0000      0.983 1.000 0.000
#&gt; GSM559384     1  0.0000      0.983 1.000 0.000
#&gt; GSM559385     1  0.0000      0.983 1.000 0.000
#&gt; GSM559386     1  0.0000      0.983 1.000 0.000
#&gt; GSM559388     2  0.0000      0.973 0.000 1.000
#&gt; GSM559389     1  0.0000      0.983 1.000 0.000
#&gt; GSM559390     1  0.0000      0.983 1.000 0.000
#&gt; GSM559392     2  0.0000      0.973 0.000 1.000
#&gt; GSM559393     1  0.0000      0.983 1.000 0.000
#&gt; GSM559394     1  0.0000      0.983 1.000 0.000
#&gt; GSM559396     1  0.0000      0.983 1.000 0.000
#&gt; GSM559398     2  0.0000      0.973 0.000 1.000
#&gt; GSM559399     1  0.0000      0.983 1.000 0.000
#&gt; GSM559400     2  0.0000      0.973 0.000 1.000
#&gt; GSM559402     1  0.0000      0.983 1.000 0.000
#&gt; GSM559403     1  0.0000      0.983 1.000 0.000
#&gt; GSM559404     1  0.0000      0.983 1.000 0.000
#&gt; GSM559405     1  0.0000      0.983 1.000 0.000
#&gt; GSM559406     1  0.0000      0.983 1.000 0.000
#&gt; GSM559407     1  0.0000      0.983 1.000 0.000
#&gt; GSM559408     1  0.0000      0.983 1.000 0.000
#&gt; GSM559409     1  0.0000      0.983 1.000 0.000
#&gt; GSM559410     1  0.0000      0.983 1.000 0.000
#&gt; GSM559411     1  0.0000      0.983 1.000 0.000
#&gt; GSM559412     1  0.0000      0.983 1.000 0.000
#&gt; GSM559413     1  0.0000      0.983 1.000 0.000
#&gt; GSM559415     1  0.1843      0.957 0.972 0.028
#&gt; GSM559416     2  0.8713      0.577 0.292 0.708
#&gt; GSM559417     1  0.8763      0.575 0.704 0.296
#&gt; GSM559418     1  0.8267      0.643 0.740 0.260
#&gt; GSM559419     1  0.0000      0.983 1.000 0.000
#&gt; GSM559420     1  0.0000      0.983 1.000 0.000
#&gt; GSM559421     2  0.0000      0.973 0.000 1.000
#&gt; GSM559423     2  0.0938      0.965 0.012 0.988
#&gt; GSM559425     2  0.0000      0.973 0.000 1.000
#&gt; GSM559426     2  0.0000      0.973 0.000 1.000
#&gt; GSM559427     2  0.0000      0.973 0.000 1.000
#&gt; GSM559428     1  0.0000      0.983 1.000 0.000
#&gt; GSM559429     2  0.2043      0.948 0.032 0.968
#&gt; GSM559430     2  0.0000      0.973 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559383     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559387     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559391     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559395     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559397     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559401     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559414     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559422     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559424     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559431     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559432     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559381     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559382     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559384     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559385     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559386     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559388     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559389     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559390     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559392     2  0.0424      0.952 0.008 0.992 0.000
#&gt; GSM559393     1  0.7671      0.314 0.568 0.052 0.380
#&gt; GSM559394     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559396     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559398     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559399     3  0.0475      0.971 0.004 0.004 0.992
#&gt; GSM559400     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559402     1  0.1289      0.933 0.968 0.000 0.032
#&gt; GSM559403     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559404     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559405     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559406     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559407     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559408     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559409     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559410     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559411     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM559412     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559413     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559415     3  0.5621      0.532 0.000 0.308 0.692
#&gt; GSM559416     2  0.3816      0.815 0.000 0.852 0.148
#&gt; GSM559417     2  0.4974      0.702 0.236 0.764 0.000
#&gt; GSM559418     2  0.4862      0.801 0.160 0.820 0.020
#&gt; GSM559419     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559420     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559421     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559423     1  0.5397      0.581 0.720 0.280 0.000
#&gt; GSM559425     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559426     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559427     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559428     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM559429     2  0.0424      0.952 0.008 0.992 0.000
#&gt; GSM559430     2  0.0000      0.956 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559383     1  0.4999    -0.5177 0.508 0.000 0.492 0.000
#&gt; GSM559387     3  0.4907     0.5639 0.420 0.000 0.580 0.000
#&gt; GSM559391     1  0.1792     0.6407 0.932 0.000 0.068 0.000
#&gt; GSM559395     3  0.4925     0.5567 0.428 0.000 0.572 0.000
#&gt; GSM559397     4  0.1637     0.7835 0.000 0.000 0.060 0.940
#&gt; GSM559401     3  0.4866     0.5729 0.404 0.000 0.596 0.000
#&gt; GSM559414     3  0.6867     0.4833 0.384 0.000 0.508 0.108
#&gt; GSM559422     3  0.4981     0.0414 0.000 0.000 0.536 0.464
#&gt; GSM559424     1  0.2149     0.6458 0.912 0.000 0.088 0.000
#&gt; GSM559431     2  0.0000     0.8099 0.000 1.000 0.000 0.000
#&gt; GSM559432     2  0.4967     0.1901 0.000 0.548 0.452 0.000
#&gt; GSM559381     4  0.0592     0.8016 0.000 0.000 0.016 0.984
#&gt; GSM559382     4  0.2530     0.7997 0.000 0.000 0.112 0.888
#&gt; GSM559384     4  0.2174     0.8121 0.052 0.000 0.020 0.928
#&gt; GSM559385     1  0.0469     0.6733 0.988 0.000 0.012 0.000
#&gt; GSM559386     4  0.0817     0.8094 0.000 0.000 0.024 0.976
#&gt; GSM559388     2  0.0927     0.8050 0.016 0.976 0.008 0.000
#&gt; GSM559389     4  0.0000     0.8064 0.000 0.000 0.000 1.000
#&gt; GSM559390     4  0.7369     0.5325 0.196 0.000 0.292 0.512
#&gt; GSM559392     2  0.2334     0.7799 0.000 0.908 0.088 0.004
#&gt; GSM559393     1  0.7641     0.2751 0.592 0.120 0.052 0.236
#&gt; GSM559394     1  0.2281     0.6328 0.904 0.000 0.096 0.000
#&gt; GSM559396     4  0.5954     0.6582 0.052 0.000 0.344 0.604
#&gt; GSM559398     2  0.0469     0.8084 0.000 0.988 0.012 0.000
#&gt; GSM559399     1  0.3484     0.5704 0.844 0.004 0.144 0.008
#&gt; GSM559400     2  0.4661     0.6772 0.016 0.728 0.256 0.000
#&gt; GSM559402     1  0.6693    -0.0181 0.488 0.000 0.088 0.424
#&gt; GSM559403     1  0.0188     0.6751 0.996 0.000 0.004 0.000
#&gt; GSM559404     4  0.2578     0.8004 0.052 0.000 0.036 0.912
#&gt; GSM559405     4  0.1109     0.7999 0.004 0.000 0.028 0.968
#&gt; GSM559406     4  0.6351     0.6824 0.104 0.000 0.268 0.628
#&gt; GSM559407     1  0.0592     0.6749 0.984 0.000 0.016 0.000
#&gt; GSM559408     4  0.5897     0.7046 0.136 0.000 0.164 0.700
#&gt; GSM559409     1  0.1182     0.6702 0.968 0.000 0.016 0.016
#&gt; GSM559410     1  0.2704     0.5398 0.876 0.000 0.124 0.000
#&gt; GSM559411     1  0.1637     0.6422 0.940 0.000 0.060 0.000
#&gt; GSM559412     4  0.2363     0.8056 0.056 0.000 0.024 0.920
#&gt; GSM559413     4  0.2385     0.8049 0.052 0.000 0.028 0.920
#&gt; GSM559415     1  0.7085     0.2886 0.568 0.232 0.200 0.000
#&gt; GSM559416     2  0.7001     0.0621 0.420 0.464 0.116 0.000
#&gt; GSM559417     2  0.8869     0.3118 0.140 0.396 0.372 0.092
#&gt; GSM559418     2  0.8311     0.3686 0.244 0.512 0.196 0.048
#&gt; GSM559419     4  0.7216     0.5597 0.156 0.000 0.336 0.508
#&gt; GSM559420     4  0.2654     0.8065 0.004 0.000 0.108 0.888
#&gt; GSM559421     2  0.0000     0.8099 0.000 1.000 0.000 0.000
#&gt; GSM559423     4  0.6508     0.4726 0.000 0.296 0.104 0.600
#&gt; GSM559425     2  0.0000     0.8099 0.000 1.000 0.000 0.000
#&gt; GSM559426     2  0.0000     0.8099 0.000 1.000 0.000 0.000
#&gt; GSM559427     2  0.0000     0.8099 0.000 1.000 0.000 0.000
#&gt; GSM559428     4  0.2408     0.8057 0.000 0.000 0.104 0.896
#&gt; GSM559429     2  0.1743     0.7814 0.000 0.940 0.004 0.056
#&gt; GSM559430     2  0.0000     0.8099 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559383     5  0.5813     0.2621 0.000 0.000 0.328 0.112 0.560
#&gt; GSM559387     3  0.3305     0.7224 0.000 0.000 0.776 0.000 0.224
#&gt; GSM559391     5  0.3241     0.7853 0.000 0.000 0.024 0.144 0.832
#&gt; GSM559395     3  0.4588     0.6721 0.000 0.000 0.720 0.060 0.220
#&gt; GSM559397     1  0.1831     0.8169 0.920 0.000 0.076 0.004 0.000
#&gt; GSM559401     3  0.2329     0.7477 0.000 0.000 0.876 0.000 0.124
#&gt; GSM559414     3  0.6077     0.3186 0.124 0.000 0.480 0.000 0.396
#&gt; GSM559422     3  0.1502     0.6884 0.056 0.000 0.940 0.004 0.000
#&gt; GSM559424     5  0.4341     0.5454 0.000 0.000 0.008 0.364 0.628
#&gt; GSM559431     2  0.0000     0.9028 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559432     3  0.3282     0.6327 0.000 0.188 0.804 0.008 0.000
#&gt; GSM559381     1  0.0451     0.8324 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559382     1  0.4002     0.7867 0.796 0.000 0.084 0.120 0.000
#&gt; GSM559384     1  0.3265     0.8109 0.844 0.000 0.012 0.128 0.016
#&gt; GSM559385     5  0.2719     0.7982 0.000 0.000 0.004 0.144 0.852
#&gt; GSM559386     1  0.2069     0.8310 0.912 0.000 0.012 0.076 0.000
#&gt; GSM559388     2  0.1851     0.8536 0.000 0.912 0.000 0.088 0.000
#&gt; GSM559389     1  0.1430     0.8356 0.944 0.000 0.004 0.052 0.000
#&gt; GSM559390     4  0.2863     0.7156 0.060 0.000 0.000 0.876 0.064
#&gt; GSM559392     2  0.2570     0.8313 0.004 0.880 0.008 0.108 0.000
#&gt; GSM559393     5  0.4058     0.7452 0.032 0.040 0.012 0.084 0.832
#&gt; GSM559394     5  0.5002     0.5759 0.000 0.000 0.052 0.312 0.636
#&gt; GSM559396     4  0.4042     0.6309 0.156 0.000 0.044 0.792 0.008
#&gt; GSM559398     2  0.1124     0.8894 0.000 0.960 0.004 0.036 0.000
#&gt; GSM559399     4  0.3336     0.5952 0.000 0.000 0.000 0.772 0.228
#&gt; GSM559400     4  0.5868     0.1638 0.000 0.380 0.104 0.516 0.000
#&gt; GSM559402     5  0.4520     0.6479 0.156 0.000 0.016 0.060 0.768
#&gt; GSM559403     5  0.2806     0.7957 0.000 0.000 0.004 0.152 0.844
#&gt; GSM559404     1  0.3068     0.7870 0.880 0.000 0.036 0.028 0.056
#&gt; GSM559405     1  0.0960     0.8267 0.972 0.000 0.016 0.004 0.008
#&gt; GSM559406     4  0.4125     0.5479 0.236 0.000 0.004 0.740 0.020
#&gt; GSM559407     5  0.2136     0.7864 0.000 0.000 0.008 0.088 0.904
#&gt; GSM559408     1  0.7048    -0.0233 0.388 0.000 0.016 0.220 0.376
#&gt; GSM559409     5  0.3134     0.7838 0.028 0.000 0.012 0.096 0.864
#&gt; GSM559410     5  0.0992     0.7416 0.000 0.000 0.024 0.008 0.968
#&gt; GSM559411     5  0.1430     0.7891 0.000 0.000 0.004 0.052 0.944
#&gt; GSM559412     1  0.2728     0.8046 0.896 0.000 0.016 0.040 0.048
#&gt; GSM559413     1  0.2095     0.8179 0.928 0.000 0.020 0.028 0.024
#&gt; GSM559415     4  0.3522     0.6100 0.000 0.004 0.004 0.780 0.212
#&gt; GSM559416     4  0.3662     0.5540 0.000 0.004 0.000 0.744 0.252
#&gt; GSM559417     4  0.2354     0.7195 0.000 0.020 0.032 0.916 0.032
#&gt; GSM559418     2  0.7510     0.0136 0.044 0.452 0.012 0.332 0.160
#&gt; GSM559419     4  0.1597     0.7219 0.012 0.000 0.000 0.940 0.048
#&gt; GSM559420     1  0.3236     0.7999 0.828 0.000 0.020 0.152 0.000
#&gt; GSM559421     2  0.0000     0.9028 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559423     1  0.6993     0.5034 0.556 0.248 0.084 0.112 0.000
#&gt; GSM559425     2  0.0000     0.9028 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559426     2  0.0162     0.9012 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559427     2  0.0000     0.9028 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559428     1  0.3586     0.8044 0.828 0.000 0.076 0.096 0.000
#&gt; GSM559429     2  0.2284     0.8263 0.096 0.896 0.000 0.004 0.004
#&gt; GSM559430     2  0.0000     0.9028 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM559383     1  0.8099   -0.02214 0.312 0.000 0.184 0.296 NA 0.032
#&gt; GSM559387     3  0.3862    0.67916 0.100 0.000 0.796 0.016 NA 0.000
#&gt; GSM559391     1  0.6137   -0.03583 0.464 0.000 0.048 0.388 NA 0.000
#&gt; GSM559395     3  0.6059    0.44150 0.144 0.000 0.584 0.216 NA 0.000
#&gt; GSM559397     6  0.3025    0.70902 0.000 0.000 0.024 0.000 NA 0.820
#&gt; GSM559401     3  0.1010    0.70331 0.036 0.000 0.960 0.000 NA 0.000
#&gt; GSM559414     3  0.6755    0.37283 0.208 0.000 0.416 0.000 NA 0.052
#&gt; GSM559422     3  0.3140    0.66698 0.000 0.000 0.844 0.024 NA 0.024
#&gt; GSM559424     4  0.5596    0.08709 0.400 0.000 0.028 0.500 NA 0.000
#&gt; GSM559431     2  0.0547    0.86971 0.000 0.980 0.000 0.000 NA 0.000
#&gt; GSM559432     3  0.2932    0.63255 0.000 0.164 0.820 0.000 NA 0.000
#&gt; GSM559381     6  0.1814    0.71236 0.000 0.000 0.000 0.000 NA 0.900
#&gt; GSM559382     6  0.3946    0.61805 0.000 0.000 0.004 0.052 NA 0.752
#&gt; GSM559384     6  0.3331    0.70058 0.004 0.000 0.000 0.044 NA 0.816
#&gt; GSM559385     1  0.2950    0.52229 0.828 0.000 0.000 0.148 NA 0.000
#&gt; GSM559386     6  0.1434    0.70838 0.000 0.000 0.000 0.012 NA 0.940
#&gt; GSM559388     2  0.4317    0.69434 0.012 0.732 0.012 0.212 NA 0.000
#&gt; GSM559389     6  0.1152    0.70553 0.000 0.000 0.000 0.004 NA 0.952
#&gt; GSM559390     4  0.4372    0.44607 0.128 0.000 0.000 0.764 NA 0.060
#&gt; GSM559392     2  0.3807    0.78322 0.000 0.808 0.000 0.048 NA 0.040
#&gt; GSM559393     1  0.6159    0.33500 0.604 0.016 0.008 0.128 NA 0.028
#&gt; GSM559394     1  0.6312    0.32171 0.592 0.004 0.104 0.180 NA 0.000
#&gt; GSM559396     4  0.5457    0.18191 0.000 0.000 0.004 0.544 NA 0.328
#&gt; GSM559398     2  0.1867    0.84738 0.000 0.916 0.000 0.020 NA 0.000
#&gt; GSM559399     4  0.4273    0.27385 0.380 0.000 0.000 0.596 NA 0.000
#&gt; GSM559400     2  0.6916    0.35995 0.004 0.492 0.076 0.252 NA 0.004
#&gt; GSM559402     1  0.6387    0.25743 0.576 0.000 0.000 0.120 NA 0.160
#&gt; GSM559403     1  0.2531    0.51376 0.856 0.000 0.000 0.132 NA 0.000
#&gt; GSM559404     6  0.4784    0.59867 0.032 0.000 0.004 0.012 NA 0.604
#&gt; GSM559405     6  0.3081    0.68298 0.004 0.000 0.000 0.000 NA 0.776
#&gt; GSM559406     4  0.6223   -0.02285 0.044 0.000 0.000 0.468 NA 0.368
#&gt; GSM559407     1  0.1536    0.55510 0.940 0.000 0.000 0.040 NA 0.004
#&gt; GSM559408     6  0.7386    0.06322 0.312 0.000 0.000 0.176 NA 0.360
#&gt; GSM559409     1  0.3637    0.51280 0.820 0.000 0.000 0.096 NA 0.032
#&gt; GSM559410     1  0.1592    0.55038 0.940 0.000 0.020 0.008 NA 0.000
#&gt; GSM559411     1  0.2007    0.54555 0.920 0.000 0.012 0.036 NA 0.000
#&gt; GSM559412     6  0.5869    0.54646 0.096 0.000 0.000 0.040 NA 0.540
#&gt; GSM559413     6  0.5512    0.59081 0.064 0.000 0.000 0.044 NA 0.588
#&gt; GSM559415     4  0.5662    0.01208 0.424 0.000 0.004 0.440 NA 0.000
#&gt; GSM559416     4  0.4315    0.33719 0.328 0.000 0.000 0.636 NA 0.000
#&gt; GSM559417     4  0.5911    0.42659 0.112 0.016 0.000 0.636 NA 0.048
#&gt; GSM559418     1  0.8204    0.00974 0.312 0.300 0.000 0.192 NA 0.048
#&gt; GSM559419     4  0.4673    0.46712 0.116 0.000 0.000 0.732 NA 0.028
#&gt; GSM559420     6  0.3384    0.65874 0.000 0.000 0.000 0.068 NA 0.812
#&gt; GSM559421     2  0.0363    0.87109 0.000 0.988 0.000 0.000 NA 0.000
#&gt; GSM559423     6  0.7132    0.20586 0.000 0.212 0.004 0.076 NA 0.392
#&gt; GSM559425     2  0.0146    0.87162 0.000 0.996 0.000 0.000 NA 0.000
#&gt; GSM559426     2  0.1219    0.86045 0.000 0.948 0.000 0.004 NA 0.000
#&gt; GSM559427     2  0.0000    0.87109 0.000 1.000 0.000 0.000 NA 0.000
#&gt; GSM559428     6  0.3078    0.64135 0.000 0.000 0.000 0.012 NA 0.796
#&gt; GSM559429     2  0.3317    0.77871 0.004 0.828 0.000 0.000 NA 0.088
#&gt; GSM559430     2  0.0547    0.87054 0.000 0.980 0.000 0.000 NA 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:NMF 52         0.723848 2
#> ATC:NMF 51         0.032758 3
#> ATC:NMF 41         0.000842 4
#> ATC:NMF 47         0.000059 5
#> ATC:NMF 32         0.000164 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0     ComplexHeatmap_2.1.1  markdown_1.1          knitr_1.26           
#>  [5] preprocessCore_1.46.0 cola_1.3.2            GEOquery_2.52.0       Biobase_2.44.0       
#>  [9] BiocGenerics_0.30.0   GetoptLong_0.1.7     
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6         matrixStats_0.55.0   bit64_0.9-7          doParallel_1.0.15   
#>  [5] RColorBrewer_1.1-2   httr_1.4.1           tools_3.6.0          backports_1.1.5     
#>  [9] R6_2.4.1             DBI_1.0.0            lazyeval_0.2.2       colorspace_1.4-1    
#> [13] withr_2.1.2          tidyselect_0.2.5     gridExtra_2.3        bit_1.1-14          
#> [17] compiler_3.6.0       xml2_1.2.2           microbenchmark_1.4-7 pkgmaker_0.28       
#> [21] slam_0.1-46          scales_1.1.0         readr_1.3.1          NMF_0.23.6          
#> [25] stringr_1.4.0        digest_0.6.23        pkgconfig_2.0.3      bibtex_0.4.2        
#> [29] highr_0.8            limma_3.40.6         rlang_0.4.2          GlobalOptions_0.1.1 
#> [33] RSQLite_2.1.2        impute_1.58.0        shape_1.4.4          mclust_5.4.5        
#> [37] dendextend_1.12.0    dplyr_0.8.3          RCurl_1.95-4.12      magrittr_1.5        
#> [41] Matrix_1.2-17        Rcpp_1.0.3           munsell_0.5.0        S4Vectors_0.22.1    
#> [45] viridis_0.5.1        lifecycle_0.1.0      stringi_1.4.3        plyr_1.8.4          
#> [49] blob_1.2.0           crayon_1.3.4         lattice_0.20-38      splines_3.6.0       
#> [53] annotate_1.62.0      circlize_0.4.9       hms_0.5.2            zeallot_0.1.0       
#> [57] pillar_1.4.2         rjson_0.2.20         rngtools_1.4         reshape2_1.4.3      
#> [61] codetools_0.2-16     stats4_3.6.0         XML_3.98-1.20        glue_1.3.1          
#> [65] evaluate_0.14        png_0.1-7            vctrs_0.2.0          foreach_1.4.7       
#> [69] polyclip_1.10-0      gtable_0.3.0         purrr_0.3.3          tidyr_1.0.0         
#> [73] clue_0.3-57          assertthat_0.2.1     ggplot2_3.2.1        xfun_0.11           
#> [77] gridBase_0.4-7       eulerr_6.0.0         xtable_1.8-4         skmeans_0.2-11      
#> [81] survival_2.44-1.1    viridisLite_0.3.0    tibble_2.1.3         iterators_1.0.12    
#> [85] AnnotationDbi_1.46.1 registry_0.5-1       memoise_1.1.0        IRanges_2.18.3      
#> [89] cluster_2.1.0        brew_1.0-6
```



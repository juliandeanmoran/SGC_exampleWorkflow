# SGC_exampleWorkflow
Example notebook for "time-nodal" spectral graph clustering of calcium imaging traces.

<p align = "center">
	<img src="./example_output/SGC_hist_edges.png", width="500" height="300">
	<img src="./example_output/SGC_KNNgraph_clustered.png", width="500" height="300">
</p>

## :brain: conceptual overview

- here we analyse a 135-neuron calcium imaging dataset for common patterns of activity
	+ all 135 activity traces are collected from the same *C. elegans* whole-brain
	+ all 135 traces have already been tracking-proofread and removed of motion, bleaching, and neighbor-invasion artifacts

- our goal is to find time windows in which "neuronal assemblies" manifest
	+ that is to say, we are looking for instances when multiple neurons seem closely coordinated in their activity

- spectral graph clustering (SGC) is an adjacency-matrix technique that leverages the graph laplacian

- "time nodal" graph clustering uses time-points as the nodes in the adjacency matrix
	+ each node is a time point, for example t=135 during the calcium imaging video
	+ each node is a 135-element array, where each element is the activity of a neuron during that time point
	+ for a description of pre-processing steps, post-processing steps, and performance, see Molter 2018 (https://bmcbiol.biomedcentral.com/articles/10.1186/s12915-018-0606-4)

- while Molter2018 provides a workable mathematical explanation...
	+ the attached Jupyter notebook shows how to implement in Python many of the steps they describe
	+ some of Molter2018's post-processing steps have been ommitted; i was unable to follow them based on the paper's limited description


## :brain: notes on the data

- this particular C. elegans is an unc-13(s69) animal
	+ meaning all chemical synaptic transmission between neurons has been perturbed; the animal is paralysed
	+ meaning gap-junctional transmission remains intact; we expect to find patterns of coordinated neuroactivity with zero time-lag
	+ therefore, we do not need to search for time-lagged patterns in this particular analysis

- notably, in this animal, neuromodulatory transmission also remains intact
	+ this can yield more complex patterns of activity between neurons
	+ to further interrogate these relationships, more standard analysis techniques may be used, e.g. cross-correlation
	+ to further interrogate these relationships, more tailored techniques may be used that evaluate for curve-transformation relationships

- most neurons in this dataset are unnamed
	+ as such, they receive a name of the format Na#, for example Na29
	+ some pharyngeal and retrovesicular neurons have been identified with high-confidence and have been named accordingly


## :seedling: setup

```
pip install -r requirements.txt
```

## :brain: conclusion

- time-nodal SGC succeeds in elucidating novel neuronal assemblies in calcium imaging data
	+ many of these are not easily discernible to the human eye by visually appraising the traces

- in this workflow, i constrained the time points that were included in the adjacency matrix (see Jupyter notebook)
	+ the analysis may benefit from including all time points, as i found that the more time points i include, the more patterns this workflow reveals
	+ graph-drawing code that depends on NetworkX must be reconfigured, as NetworkX throws errors when drawing >500 nodes in the graph

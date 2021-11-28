# My Project
Phylogenetic approaches, in which statistical models are fit to genealogies constructed from pathogen
genomes, have recently become an important source of information on the transmission dynamics of diseases. 
The [Pennell lab](https://www.zoology.ubc.ca/person/matthew-pennell) has recently discovered a statistical issue that suggests that in some cases, this
phylogenetic information may be misleading. 

In order to study this potential discrepancy between the actual and reconstructed epidemic, I will utilize FAVITES to simulate viral transmissions.
The viral sequence generated will subsequently be used to reconstruct the epidemic in BEAST2. The examination of the reconstruction and prediction processes will 
shed light on how well and accurate the current methods work.


# FAVITES Simulation

**FAVITES** (FrAmework for VIral Transmission and Evolution Simulation) is a robust modular framework for simulation of viral transmissions and sequence evolution 
([Moshiri *et al*., 2018](https://doi.org/10.1093/bioinformatics/bty921)). By breaking the interaction network down to nodes and edges, the users can easily customize
the simulation process by parameterizing the module classes. 

All the installation information, requirements, usage, file format descriptions, etc., are located in the [FAVITES Wiki](https://github.com/niemasd/FAVITES/wiki).

# Module Implementations
In this section, we will discuss the module implementations important for simulating the epidemic.

1. *ContactNetworkGenerator*
   - Creates an edge list representung the input contact network (either by reading it from file or generating it)
   - Uses in conjunction with *ContactNetworkEdge* and *ContactNetworkNode*
   - **Barabasi-Albert Model** used for implementation
     - A graph of n nodes is grown by attaching new nodes each with m edges that are preferentially attached to existing nodes with high degree
     - Config parameters: **num_cn_nodes** and **num_edges_from_new**


2. *End Criteria*
   - Determines whether or not the simulation process should terminate at any given moment
   - **FirstTimeTransmissions** used for implementation
     - Users can specify both an end time and a number of transmissions as ending criteria, and the first to be reached will end the simulation
     - Config parameters: **end_time** and **end_transmissions**
   - Other implementations such as GEMF are not used here to avoid complexity 

3. *NodeEvolution*
   - Evolves the phylogenetic trees of the viruses within a specified node until a specified time
   - **VirusTreeSimulator** used for implementation
     - Evolves viral phylogenetic trees under a coalescent model with various effective population growth models: constant, exponental or logistic
     - **vts_model**: sets the desired viral population growth model to use (constant, exponential, logistic)
     - **vts_n0**: sets the desired effective population size at time zero, 1 is utilized if you assume a single viral was transmitted
     - **vts_growthRate**: sets the desired effective population size growth rate (only used in exponential and logistic models)
     - **vts_max_attempts**: sets the maximum number of attempts to coalescence a single tree (e.g. 100) before FAVITES kills VirusTreeSimulator

4. *SequenceEvolution*
   - Evolves the sequences of the viruses within a specified node until a specified time
   - Lots of implementations can be appropriate here, to be consistent with previous studies, the substitution rates for the HIV transmission were used as input here
 
5. *TransmissionNodeSample*
   - Chooses two nodes to be involved in a given transmission event
   - **RandomSingleTransmission** is used here to avoid complexity 

Besides parameters specific to the epidemic itself, the real-life sampling effort can also be designed via the following modules.

6. **NodeAvailability**
7. **NumBranchSample**
8. **Sequencing**
9. **TimeSample**


Other module implementations are also required to be specified in the config file to provide coherent instructions to the simulation. However, only the ones mentioned above have crucial biological meaning for my project (and were thus discussed in detail). More information on the complete list of modules can be found in the the [FAVITES Wiki](https://github.com/niemasd/FAVITES/wiki).



 

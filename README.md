SARship detection remains challenging due toweak small-
target responses, complex near-shore backgrounds, and the difficulty of
modeling elongated ship structureswith homogeneous prediction heads.
In addition, unified box regression constraints are often insufficient to
match the distinct localization roles of different prediction layers. To
address these issues, this paper proposes STMH-Net, a lightweight het-
erogeneous dual-head network built uponYOLOv5s for SARship detec-
tion. Specifically, a Spatial-Texture Perception (STP)BlockwithDual-
PoolingChannelAttention(DPCA) is introducedintothehigh-resolution
branchto enhance shallowtexture representationanddirectional context
modeling forweak small ships.AMorphology-Aware (MA)Head is fur-
theremployedinthemedium-resolutionbranchtostrengthenmorphology-
sensitive aggregation for elongated ship targets. In addition, a Head-
aligned Scale-MorphologyBox Loss (HSM-Box Loss) is designed to im-
pose branch-aligned regression constraints for different prediction layers.
Experimental results show that STMH-Net achieves 95.1% Precision,
89.0% Recall, 93.4% mAP@0.5, and 73.9% mAP@0.5:0.95 on HRSID,
improvingmAP@0.5:0.95 by 3.5 points over the baselinewith only 7.5M
parameters.On SSDD, the proposedmethod attains the best Recall of
97.1%whilemaintaining competitive performance on the othermetrics.
Overall, its comprehensive performance is superior to that ofmost base-
linemodels,demonstrating thatSTMH-Netprovides aneffectivebalance
among detection accuracy, localization quality, andmodel efficiency for
SARship detection.

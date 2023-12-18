<div align="center">
<a href="http://camma.u-strasbg.fr/">
<img src="logo.png" width="50%">
</a>
</div>

# The Endoscapes Dataset for Surgical Scene Segmentation, Object Detection, and Critical View of Safety Assessment

<div align="center">
<img src="endoscapes_examples.png" width="100%">
</div>

## Overview

We are excited to release Endoscapes2023 \[[Download](https://seafile.unistra.fr/f/5f1503749bb644bdbd9f/?dl=1)\], a comprehensive laparoscopic video dataset for surgical anatomy and tool segmentation, object detection, and Critical View of Safety (CVS) assessment.
This repository provides an overview of the dataset contents, including an exploration of the types and format of the annotations as well as download links.

## Contents
**Endoscapes2023** focuses on a region of interest within laparoscopic cholecystectomy videos where CVS is relevant and well-defined: during the dissection phase and before the first clip/cut of the cystic artery or cystic duct. We organize its contents into three different sub-datasets:
1) _Endoscapes-CVS201_: 11090 frames from 201 videos annotated with CVS by 3 experts. These frames are evenly spaced at 5 second intervals, and the intermediate frames can be used to train for instance semi-supervised/temporal methods. There are a total of 58813 frames (1 fps) in the aforementioned region of interest.

2) _Endoscapes-BBox201_: 1933 frames from 201 videos annotated with bounding boxes for 5 anatomical structures/regions (Gallbladder, Cystic Duct, Cystic Artery, Cystic Plate, Hepatocystic Triangle Dissection) and a tool class (6 classes total). The 1933 frames correspond to 1 frame every 30 seconds from the aformentioned region of interest of each video. As for Endoscapes-CVS201, the unlabeled frames can be used for semi-supervised/temporal methods. 

3) _Endoscapes-Seg50_: 493 frames from 50 videos annotated with instance and semantic segmentation masks for the 6 aforementioned classes. Endoscapes-Seg50 is a strict subset of Endoscapes-BBox201; we select ~25% of the 201 videos (50), sample 1 frame every 30s from the region of interest, and add segmentation annotations.

## File Structure
We describe the file structure below. All annotations are in COCO-format, with CVS labels encoded as image-level tags. Of note, the CVS labels represent the average of the 3 annotators for each criterion. Decimal values indicate cases where there was disagreement among annotators.

```shell
$DATA_HOME
└── train
    ├── 1_29375.jpg # SYNTAX: ${VIDEO_ID}_{FRAME_NUM}.jpg
    ├── ...
    ├── 120_85800.jpg
    ├── annotation_coco.json # Annotation File for Endoscapes-BBox201 (Object Detection, COCO Format). Also contains instance segmentation where available.
    ├── annotation_coco_vid.json # Annotation File for Endoscapes-CVS201 Temporal Models. Contains Bounding Box/Segmentation where available.
    ├── annotation_ds_coco.json # Annotation File for Endoscapes-CVS201 Single-Frame Models. Contains Bounding Box/Segmentation where available.
└── val
    ├── 121_14850.jpg
    ├── ...
    ├── 161_34325.jpg
    ├── annotation_coco.json
    ├── annotation_coco_vid.json
    ├── annotation_ds_coco.json
└── test
    ├── 162_5850.jpg
    ├── ...
    ├── 201_46125.jpg
    ├── annotation_coco.json
    ├── annotation_coco_vid.json
    ├── annotation_ds_coco.json
└── train_seg
    ├── 4_21725.jpg
    ├── ...
    ├── 119_58000.jpg
    ├── annotation_coco.json # Annotation File for Endoscapes-Seg50 (bounding boxes + instance segmentation, COCO format)
└── val_seg
    ├── 126_10825.jpg
    ├── ...
    ├── 159_60875.jpg
    ├── annotation_coco.json
└── test_seg
    ├── 165_22925.jpg
    ├── ...
    ├── 189_34875.jpg
    ├── annotation_coco.json
└── 12_5 # Official 12.5% Splits of Endoscapes-CVS201 Train Set (3 folds)
    ├── train_0
        ├── 16_9500.jpg
        ├── ...
        ├── 118_121800.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
    ├── train_1
        ├── 2_29075.jpg
        ├── ...
        ├── 107_57875.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
    ├── train_2
        ├── 8_14050.jpg
        ├── ...
        ├── 119_90825.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
└── 25 # Official 25% Splits of Endoscapes-CVS201 Train Set (3 folds)
    ├── train_0
        ├── 1_29375.jpg
        ├── ...
        ├── 116_36150.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
    ├── train_1
        ├── 2_29075.jpg
        ├── ...
        ├── 120_85800.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
    ├── train_2 # Same videos as train_seg above
        ├── 4_21725.jpg
        ├── ...
        ├── 119_58000.jpg
        ├── annotation_coco.json
        ├── annotation_coco_vid.json
        ├── annotation_ds_coco.json
└── all_metadata.csv # all metadata for the entire Endoscapes2023 (CVS annotations from all annotators, etc.)
└── insseg
    ├── 5_5900.npy # stack of instance masks, where each channel corresponds to an object instance
    ├── 5_5900.csv # label of each slice of numpy masks
    ├── ...
└── semseg
    ├── 5_5900.png # semantic segmentation mask, 480x854x3 image with each element representing the class_id of each pixel; the 3 channels are identical
    ├── ...
└── train_vids.txt # 120 training videos for Endoscapes-CVS201 and Endoscapes-BBox201
└── val_vids.txt # 41 validation videos for Endoscapes-CVS201 and Endoscapes-BBox201
└── test_vids.txt # 40 testing videos for Endoscapes-CVS201 and Endoscapes-BBox201
└── train_seg_vids.txt # 30 training videos for Endoscapes-Seg50
└── val_seg_vids.txt # 10 validation videos for Endoscapes-Seg50
└── test_seg_vids.txt # 10 testing videos for Endoscapes-Seg50
└── seg_label_map.txt # map from class name to id; note that background is ignored for object detection/instance segmentation; for these tasks, id 0 becomes cystic plate, ...
```

## Baselines/Benchmark

We have published a technical report [1] alongside this repository wherein we present a thorough benchmark for three tasks: object detection (using Endoscapes-BBox201), instance segmentation (using Endoscapes-Seg50), and CVS prediction (using Endoscapes-CVS201). This benchmark also includes comparisons to recently published works.

Please refer to [this repository](https://github.com/CAMMA-public/SurgLatentGraph/) to run all models presented in [1].

## License
The data released here are available for non-commercial scientific research purposes as defined in the [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). By downloading and using this code you agree to the terms in the [LICENSE](LICENSE). Third-party codes are subject to their respective licenses.

## Acknowledgement
This work was supported by French state funds managed by the ANR within the National AI Chair program under Grant ANR-20-CHIA-
0029-01 (Chair AI4ORSafety) and within the Investments for the future program under Grants ANR-10-IDEX-0002-02 (IdEx Unistra) and ANR-
10-IAHU-02 (IHU Strasbourg). This work was granted access to the HPC resources of IDRIS under the allocation 2021-AD011011640R1
made by GENCI.

## Contact
This dataset is maintained by the research group [CAMMA](http://camma.u-strasbg.fr). If you have any questions regarding the dataset, please create a GitHub issue and we will get back to you as soon as we can.

## References

Please cite the following works when using this dataset for your research.

(arXiv)
(NSD)

If you use Endoscapes-CVS201 or Endoscapes-BBox201, please additionally cite the following:
```bibtex
@article{murali2023latent,
  author={Murali, Aditya and Alapatt, Deepak and Mascagni, Pietro and Vardazaryan, Armine and Garcia, Alain and Okamoto, Nariaki and Mutter, Didier and Padoy, Nicolas},
  journal={IEEE Transactions on Medical Imaging},
  title={Latent Graph Representations for Critical View of Safety Assessment}, 
  year={2023},
  volume={},
  number={},
  pages={1-1},
  doi={10.1109/TMI.2023.3333034}
}
```

And if you use Endoscapes-Seg50, please additionally cite:
```bibtex
@article{alapatt2021temporally,
  title={Temporally Constrained Neural Networks (TCNN): A framework for semi-supervised video semantic segmentation},
  author={Alapatt, Deepak and Mascagni, Pietro and Vardazaryan, Armine and Garcia, Alain and Okamoto, Nariaki and Mutter, Didier and Marescaux, Jacques and Costamagna, Guido and Dallemagne, Bernard and Padoy, Nicolas},
  journal={arXiv preprint arXiv:2112.13815},
  year={2021}
}
```

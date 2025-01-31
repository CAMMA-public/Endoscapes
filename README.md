<div align="center">
<a href="http://camma.u-strasbg.fr/">
<img src="logo.png" width="50%">
</a>
</div>

# The Endoscapes Dataset for Surgical Scene Segmentation, Object Detection, and Critical View of Safety Assessment

<div align="center">
<img src="endoscapes_examples.png" width="100%">
</div>

[![Endoscapes Dataset and COCO Style Annotations](https://img.shields.io/badge/Endoscapes-Download-green)](https://s3.unistra.fr/camma_public/datasets/endoscapes/endoscapes.zip)
[![arXiv](https://img.shields.io/badge/arXiv-2312.12429-red)](https://arxiv.org/abs/2312.12429)

## Overview

We are excited to release Endoscapes2023 \[[Download](https://s3.unistra.fr/camma_public/datasets/endoscapes/endoscapes.zip)\], a comprehensive laparoscopic video dataset for surgical anatomy and tool segmentation, object detection, and Critical View of Safety (CVS) assessment.
This repository provides an overview of the dataset contents, including an exploration of the types and format of the annotations as well as download links. Comprehensive information, including dataset splits and performance benchmarks, is provided in this accompanying [technical report](https://arxiv.org/abs/2312.12429).

## Contents
**Endoscapes2023** focuses on a region of interest within laparoscopic cholecystectomy videos where CVS is relevant and well-defined: during the dissection phase and before the first clip/cut of the cystic artery or cystic duct [4]. We organize its contents into three different sub-datasets:
1) _Endoscapes-CVS201_: 11090 frames from 201 videos annotated with CVS by 3 experts. These frames are evenly spaced at 5 second intervals, and the intermediate frames can be used to train for instance semi-supervised/temporal methods. There are a total of 58813 frames (1 fps) in the aforementioned region of interest.

2) _Endoscapes-BBox201_: 1933 frames from 201 videos annotated with bounding boxes for 5 anatomical structures/regions (Gallbladder, Cystic Duct, Cystic Artery, Cystic Plate, Hepatocystic Triangle Dissection) and a tool class (6 classes total). The 1933 frames correspond to 1 frame every 30 seconds from the aformentioned region of interest of each video. As for Endoscapes-CVS201, the unlabeled frames can be used for semi-supervised/temporal methods. 

3) _Endoscapes-Seg50_: 493 frames from 50 videos annotated with instance and semantic segmentation masks for the 6 aforementioned classes. Endoscapes-Seg50 is a strict subset of Endoscapes-BBox201; we select ~25% of the 201 videos (50 out of 201), sample 1 frame every 30s from the region of interest, and add segmentation annotations. **Please note that this is a partial release of the dataset used in [2,3,5]; these works use _Endoscapes-Seg201_ (private), which contains segmentation masks for all 1933 frames that are a part of _Endoscapes-BBox201_.**

## File Structure
We describe the file structure below. All annotations are in COCO-format, with CVS labels encoded as image-level tags. Of note, the CVS labels represent the average of the 3 annotators for each criterion. Decimal values indicate cases where there was disagreement among annotators.

```shell
$endoscapes
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

## Code/Baselines/Benchmark

We have published a technical report [1] alongside this repository wherein we present a thorough benchmark for three tasks: object detection (using Endoscapes-BBox201), instance segmentation (using Endoscapes-Seg50), and CVS prediction (using Endoscapes-CVS201). This benchmark also serves to centralize numerous results that have been reported in several recently published works [2,5,6].

Please refer to [this repository](https://github.com/CAMMA-public/SurgLatentGraph/) for instructions and code examples for loading/using the dataset as well as for code and instructions to run all models presented in [1]. 

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

Please cite the accompanying technical report when using this dataset for your research.

[\[1\]](https://arxiv.org/abs/2312.12429) Murali, A., Alapatt, D., Mascagni, P., Vardazaryan, A., Garcia, A., Okamoto, N., ... & Padoy, N. (2023). The Endoscapes Dataset for Surgical Scene Segmentation, Object Detection, and Critical View of Safety Assessment: Official Splits and Benchmark. arXiv preprint arXiv:2312.12429.
```bibtex
@article{murali2023endoscapes,
      title={The Endoscapes Dataset for Surgical Scene Segmentation, Object Detection, and Critical View of Safety Assessment: Official Splits and Benchmark}, 
      author={Aditya Murali and Deepak Alapatt and Pietro Mascagni and Armine Vardazaryan and Alain Garcia and Nariaki Okamoto and Guido Costamagna and Didier Mutter and Jacques Marescaux and Bernard Dallemagne and Nicolas Padoy},
      journal={arXiv preprint arXiv:2312.12429},
      year={2023},
}
```

If you use Endoscapes-CVS201 or Endoscapes-BBox201, please additionally cite the following works which led to the creation of these datasets:

[\[2\]](https://arxiv.org/abs/2212.04155) Murali, A., Alapatt, D., Mascagni, P., Vardazaryan, A., Garcia, A., Okamoto, N., ... & Padoy, N. (2023). Latent graph representations for critical view of safety assessment. IEEE Transactions on Medical Imaging.
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

Similarly, if you Endoscapes-Seg50, please additionally cite the following work which led to the generation of the segmentation annotations:

[\[3\]](https://arxiv.org/abs/2112.13815) Alapatt, D., Mascagni, P., Vardazaryan, A., Garcia, A., Okamoto, N., Mutter, D., ... & Padoy, N. (2021). Temporally Constrained Neural Networks (TCNN): A framework for semi-supervised video semantic segmentation. arXiv preprint arXiv:2112.13815.
And if you use Endoscapes-Seg50, please additionally cite:
```bibtex
@article{alapatt2021temporally,
  title={Temporally Constrained Neural Networks (TCNN): A framework for semi-supervised video semantic segmentation},
  author={Alapatt, Deepak and Mascagni, Pietro and Vardazaryan, Armine and Garcia, Alain and Okamoto, Nariaki and Mutter, Didier and Marescaux, Jacques and Costamagna, Guido and Dallemagne, Bernard and Padoy, Nicolas},
  journal={arXiv preprint arXiv:2112.13815},
  year={2021}
}
```

For a detailed description of the annotation protocols used to generate this dataset, please see:

[\[4\]](https://arxiv.org/abs/2106.10916) Mascagni, P., Alapatt, D., Garcia, A., Okamoto, N., Vardazaryan, A., Costamagna, G., ... & Padoy, N. (2021). Surgical data science for safe cholecystectomy: a protocol for segmentation of hepatocystic anatomy and assessment of the critical view of safety. arXiv preprint arXiv:2106.10916.
```bibtex
@article{mascagni2021surgical,
  title={Surgical data science for safe cholecystectomy: a protocol for segmentation of hepatocystic anatomy and assessment of the critical view of safety},
  author={Mascagni, Pietro and Alapatt, Deepak and Garcia, Alain and Okamoto, Nariaki and Vardazaryan, Armine and Costamagna, Guido and Dallemagne, Bernard and Padoy, Nicolas},
  journal={arXiv preprint arXiv:2106.10916},
  year={2021}
}
```

Finally, below is a list of additional works that use this dataset:

[\[5\]](https://arxiv.org/abs/2312.06829) Murali, A., Alapatt, D., Mascagni, P., Vardazaryan, A., Garcia, A., Okamoto, N., ... & Padoy, N. (2023, October). Encoding Surgical Videos as Latent Spatiotemporal Graphs for Object and Anatomy-Driven Reasoning. In International Conference on Medical Image Computing and Computer-Assisted Intervention (pp. 647-657). Cham: Springer Nature Switzerland.
```bibtex
@article{murali2023encoding,
  title={Encoding Surgical Videos as Latent Spatiotemporal Graphs for Object and Anatomy-Driven Reasoning},
  author={Murali, Aditya and Alapatt, Deepak and Mascagni, Pietro and Vardazaryan, Armine and Garcia, Alain and Okamoto, Nariaki and Mutter, Didier and Padoy, Nicolas},
  journal={arXiv preprint arXiv:2312.06829},
  year={2023}
}
```

[\[6\]](https://arxiv.org/abs/2312.05968) Alapatt, D., Murali, A., Srivastav, V., Mascagni, P., Consortium, A., & Padoy, N. (2023). Jumpstarting Surgical Computer Vision. arXiv preprint arXiv:2312.05968.
```bibtex
@article{alapatt2023jumpstarting,
  title={Jumpstarting Surgical Computer Vision},
  author={Alapatt, Deepak and Murali, Aditya and Srivastav, Vinkle and Mascagni, Pietro and Consortium, AI4SafeChole and Padoy, Nicolas},
  journal={arXiv preprint arXiv:2312.05968},
  year={2023}
}
```

[\[7\]](https://arxiv.org/pdf/2207.00449) Ramesh, S., Srivastav, V., Alapatt, D., Yu, T., Murali, A., Sestini, L., ... & Padoy, N. (2023). Dissecting self-supervised learning methods for surgical computer vision. Medical Image Analysis, 88, 102844.
```bibtex
@article{ramesh2023dissecting,
  title={Dissecting self-supervised learning methods for surgical computer vision},
  author={Ramesh, Sanat and Srivastav, Vinkle and Alapatt, Deepak and Yu, Tong and Murali, Aditya and Sestini, Luca and Nwoye, Chinedu Innocent and Hamoud, Idris and Sharma, Saurav and Fleurentin, Antoine and others},
  journal={Medical Image Analysis},
  volume={88},
  pages={102844},
  year={2023},
  publisher={Elsevier}
}
```

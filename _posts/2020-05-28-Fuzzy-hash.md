# Fuzzy hash in software detection

**Experimental Study of Fuzzy Hashing in Malware Clustering Analysi**

Usenix Security, workshop CEST

 

For instance, ShadowServer [1] started to use fuzzy hash functions for malware similarity analysis in 2007. Around 2012, VirusTotal [2] started to generate an ssdeep [18] hash value for each submitted malware sample. Unfortunately, these attempts have not been rigorously validated, even though several blog posts talk about failing to apply fuzzy hashing algorithms in detecting similar executable samples [12].

ModSecurity [4] introduces a new operator [3] that uses fuzzy hashing to identify web-based
malware in web attachment uploads.



Fuzzy Hash 的局限

- 固定长度。固定长度的hash值意味着大文件和小文件有着不同的压缩率，因此比较相似性时意义不大。





**BitShred: Feature Hashing Malware for Scalable Triage and Semantic Analysis**

BitShred’s job is to speed up subsequent correlation after using existing techniques to perform per-sample feature extraction. In our implementation, we experiment with using n-grams as proposed in [9, 25, 38] because the feature space is extremely large, and dynamic behavior analysis from Bayer et al. [13] because it has been shown effective.





上两片工作针对的是使用fuzzy hash 进行加速。而对于特征的选取并不是工作重点，以下内容为上两篇工作中，使用的特征的论文出处以及工作方法相关内容。




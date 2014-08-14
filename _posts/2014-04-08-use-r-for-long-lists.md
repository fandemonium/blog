---
layout: post
title: Matching ID's for Tables in R
categories: R
tags: productivity  
---
I am a newbie when it comes to lots of things, including python and R. I can use them but I cannot make them do magic in the most efficient way... I always use python to parse files (e.g., get rid of redundant lines/columns, insert tabs, matching ID's, etc.) and get them into the desired format before I use R for downstream analysis (e.g., statistics, plotting, etc.). It worked well until recentrly when I was trying to connect ID's in UniprotKB with my assembly ID's. The UniprotKB ID list was simply too large for python dictionary to work efficiently (and it could also simiply be that I didn't know other ways to use python more efficiently). Regardless, I decided to give it a go in R and surprisingly (to me), R memory handled it really well. Below is some of the codes and examples (mostly a note of self for next time). 

<!--more-->

---
uniprokb<-read.delim("~/Documents/Databases/uniprotKB_id_microbes.txt", header=F)

head(uniprokb)
---
      >      V1       V2   
      >1 P21215    Bacteria   
      >2 P80438    Bacteria   
      >3 Q8GBW6    Bacteria   
      >4 O42766    Fungi   
      >5 Q8SW28    Fungi   
      >6 Q99002    Fungi   

```R
Asp<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/Asp/ref_aligned.faa.ref.list", header=F, sep=" ")

head(Asp)
```
>\>                      V1    V2  V3        V4    
>\>1 \>K1VXR2\_TRIAC/235-496 [subseq from] K1VXR2\_TRIAC   
>\>2 \>K1VXR2\_TRIAC/536-823 [subseq from] K1VXR2\_TRIAC   
>\>3 \>F5HFH8\_CRYNB/126-436 [subseq from] F5HFH8\_CRYNB   
>\>4 \>Q5KNQ9\_CRYNJ/126-436 [subseq from] Q5KNQ9\_CRYNJ    
>\>5 \>J9VH59\_CRYNH/126-436 [subseq from] J9VH59\_CRYNH   
>\>6 \>S7QJL1\_GLOTA/96-407 [subseq from] S7QJL1\_GLOTA   
>

```R
Asp.id<-data.frame(do.call('rbind', strsplit(as.character(Asp$V4), '_', fixed=T)))
head(Asp.id)
```

>\>      X1    X2  
>\>1 K1VXR2 TRIAC   
>\>2 K1VXR2 TRIAC   
>\>3 F5HFH8 CRYNB   
>\>4 Q5KNQ9 CRYNJ    
>\>5 J9VH59 CRYNH   
>\>6 S7QJL1 GLOTA   
>

```R
asp.tax<-cbind(Asp.id, uniprokb$V2[match(Asp.id$X1, uniprokb$V1)])   
head(asp.tax)   
```
>\>      X1    X2 uniprokb$V2[match(Asp.id$X1, uniprokb$V1)]   
>\>1 K1VXR2 TRIAC                                      Fungi   
>\>2 K1VXR2 TRIAC                                      Fungi   
>\>3 F5HFH8 CRYNB                                      Fungi   
>\>4 Q5KNQ9 CRYNJ                                      Fungi   
>\>5 J9VH59 CRYNH                                      Fungi   
>\>6 S7QJL1 GLOTA                                      Fungi  
> 
>> asp.na<-subset(asp.tax, is.na(asp.tax[, 3]))
>> head(asp.na)
>        X1    X2 uniprokb$V2[match(Asp.id$X1, uniprokb$V1)]
>89    CARP TRIVH                                       <NA>
>108   CARP ARTBC                                       <NA>
>111   CARP NEUCR                                       <NA>
>129   CARP YEAST                                       <NA>
>174 E7RBI7 OGAPD                                       <NA>
>206  CARPV CANAX                                       <NA>
> 
>> ref_asp<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/Asp/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_m1<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/M1/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_m14<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/M14/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_m28<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/M28/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_m4c<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/M4_C/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_s10<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/S10/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_s8<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/S8/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_u56<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/U56/ref_aligned.faa.ref.list", header=F, sep=" ")
>> ref_tryp<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/trypsin/ref_aligned.faa.ref.list", header=F, sep=" ")
>
>> head(ref_asp)
>                     V1      V2    V3           V4
>1 >K1VXR2_TRIAC/235-496 [subseq from] K1VXR2_TRIAC
>2 >K1VXR2_TRIAC/536-823 [subseq from] K1VXR2_TRIAC
>3 >F5HFH8_CRYNB/126-436 [subseq from] F5HFH8_CRYNB
>4 >Q5KNQ9_CRYNJ/126-436 [subseq from] Q5KNQ9_CRYNJ
>5 >J9VH59_CRYNH/126-436 [subseq from] J9VH59_CRYNH
>6  >S7QJL1_GLOTA/96-407 [subseq from] S7QJL1_GLOTA

> ref_asp.id<-data.frame(do.call("rbind", strsplit(as.character(ref_asp$V1), ">|\\_|\\/")))
> head(ref_asp.id)
  X1     X2    X3      X4
1    K1VXR2 TRIAC 235-496
2    K1VXR2 TRIAC 536-823
3    F5HFH8 CRYNB 126-436
4    Q5KNQ9 CRYNJ 126-436
5    J9VH59 CRYNH 126-436
6    S7QJL1 GLOTA  96-407

ref_pro<-c(as.character(ref_asp.id$X3), as.character(ref_m1.id$X3), as.character(ref_m14.id$X3), as.character(ref_m28.id$X3), as.character(ref_m4c.id$X3), as.character(ref_s10.id$X3), as.character(ref_s8.id$X3), as.character(ref_u56.id$X3), as.character(ref_tryp.id$X3))
> ref_rpob_pro<-vector(mode="list")
> ref_rpob_pro$ref_proteases_org<-unique(ref_pro)
> ref_rpob_pro$ref_rpoB_org<-unique(ref_rpob.id$X1)
> V<-Venn(ref_rpob_pro)
> pdf("ref_rpob_pro_org_venn.pdf")
> plot(V, doWeight=F)
> dev.off()
pdf 
  2 


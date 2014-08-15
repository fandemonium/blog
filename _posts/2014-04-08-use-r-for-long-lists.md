---
layout: post
title: Matching ID's for Tables in R
categories: R
tags: productivity, data manipulation
---
I am a newbie when it comes to lots of things, including python and R. I can use them but I cannot make them do magic in the most efficient way... I always use python to parse files (e.g., get rid of redundant lines/columns, insert tabs, matching ID's, etc.) and get them into the desired format before I use R for downstream analysis (e.g., statistics, plotting, etc.). It worked well until recentrly when I was trying to connect ID's in UniprotKB with my assembly ID's. The UniprotKB ID list was simply too large for python dictionary to work efficiently (and it could also simiply be that I didn't know other ways to use python more efficiently). Regardless, I decided to give it a go in R and surprisingly (to me), R memory handled it really well. Below is some of the codes and examples (mostly a note to self for next time). 

<!--more-->
> **Note:** This method eventually didn't work out because UniProt fasta file contains sequence ID's (some are old ID's) that might be different from the BioPython queried UniProtKB dat file ID's. But the method is valid for anything with matched ID's.

Prerequisites: 
--------------

+   A tab delimited file containing sequence ID and taxa (parsed by using biopython)   
+   Other tab delimited files that need to be compared (each file was extracted from fasta files by using grep)   
+   Install Vennerable from source

Objectives:
-----------

+   Confirm some of the genes are bacterial only (or fungi only). 
+   Check how many genes overlap between groups (e.g., how many protease genes belong the organisms that were found in rpoB gene file)

How to do it:
-------------

1.  Read in the super long list of ID's queried from UniprotKB dat file.   

    ~~~
    uniprokb<-read.delim("~/Documents/Databases/uniprotKB_id_microbes.txt", header=F)
    ~~~
  
    1.  Do a quick check on the table imported.   
                               
        ```
        > head(uniprokb)
        >      V1       V2   
        >1 P21215    Bacteria   
        >2 P80438    Bacteria   
        >3 Q8GBW6    Bacteria   
        >4 O42766    Fungi   
        >5 Q8SW28    Fungi   
        >6 Q99002    Fungi   
        ```

2.  Read in the list of genes queried from the gene fasta file (Asp in the beneath example).
 
    ~~~
    ref_asp<-read.table("/Users/metagenomics/Documents/Fan/scratch/pfam_done/ToAnalyze/Asp/ref_aligned.faa.ref.list", header=F, sep=" ")
    ~~~
    
    1.  Also do a quick check on the table imported.  
      
        ```
        > head(ref_asp)
        >                     V1      V2    V3           V4  
        >1 >K1VXR2_TRIAC/235-496 [subseq from] K1VXR2_TRIAC
        >2 >K1VXR2_TRIAC/536-823 [subseq from] K1VXR2_TRIAC
        >3 >F5HFH8_CRYNB/126-436 [subseq from] F5HFH8_CRYNB
        >4 >Q5KNQ9_CRYNJ/126-436 [subseq from] Q5KNQ9_CRYNJ
        >5 >J9VH59_CRYNH/126-436 [subseq from] J9VH59_CRYNH
        >6  >S7QJL1_GLOTA/96-407 [subseq from] S7QJL1_GLOTA
        ```

    2.  As it's observed, the sequence ID's need to be parsed.

        ~~~     
        ref_asp.id<-data.frame(do.call("rbind", strsplit(as.character(ref_asp$V1), ">|\\_|\\/")))
        ~~~

    3.  This is what it would look like after splitting

        ```
        > head(ref_asp.id)
        >  X1     X2    X3      X4
        >1    K1VXR2 TRIAC 235-496
        >2    K1VXR2 TRIAC 536-823
        >3    F5HFH8 CRYNB 126-436
        >4    Q5KNQ9 CRYNJ 126-436
        >5    J9VH59 CRYNH 126-436
        >6    S7QJL1 GLOTA  96-407        
        ```

3.  Repeat Step 2 for additional files that needs to be checked.

4.  Now, there are a couple of things we could do. 
    1.  First of all, we could check what are the taxa distribution of each gene. 
       1.  This could be done by using `match`: matching ID's in ref_asp.id (ref_asp.id$X2) to ID's in uniprotkb (uniprotkb$v1), then query the matched taxa from uniprotkb (uniprokb$V2).

           ~~~
           ref_asp.tax<-cbind(Asp.id, uniprokb$V2[match(ref_asp.id$X2, uniprokb$V1)])   
           ~~~
       
       2.  This is what the new dataframe would look like:

           ```
           > head(ref_asp.tax)
           >      X1    X2 uniprokb$V2[match(ref_asp.id$X1, uniprokb$V1)]   
           >1 K1VXR2 TRIAC                                      Fungi   
           >2 K1VXR2 TRIAC                                      Fungi   
           >3 F5HFH8 CRYNB                                      Fungi   
           >4 Q5KNQ9 CRYNJ                                      Fungi   
           >5 J9VH59 CRYNH                                      Fungi   
           >6 S7QJL1 GLOTA                                      Fungi  
           ```
       
       3.  To see if there are any unmatched ID's:

           ~~~
           asp.na<-subset(ref_asp.tax, is.na(asp.tax[, 3]))
           ~~~

           Which would look like this:

           ```
           > head(asp.na)
           >        X1    X2 uniprokb$V2[match(ref_asp.id$X1, uniprokb$V1)]
           >89    CARP TRIVH                                       <NA>
           >108   CARP ARTBC                                       <NA>
           >111   CARP NEUCR                                       <NA>
           >129   CARP YEAST                                       <NA>
           >174 E7RBI7 OGAPD                                       <NA>
           >206  CARPV CANAX                                       <NA>
           ```
       4.  One can check the number of unmatched ID's by `length(asp.na[, 1])`
    
    2.  Besides the individual file check, one can also check the overlaps between multiple files by using Venn diagrams (use `library(Vennerable)`).
        1.  For example, for files like `ref_asp.id`, one can create a new data frame containing all of the organism acronyms (e.g., `ref_asp.id$x3`) 

            ~~~
            ref_pro<-c(as.character(ref_asp.id$X3), as.character(ref_m1.id$X3), as.character(ref_m14.id$X3), as.character(ref_m28.id$X3), as.character(ref_m4c.id$X3), as.character(ref_s10.id$X3), as.character(ref_s8.id$X3), as.character(ref_u56.id$X3), as.character(ref_tryp.id$X3))
            ~~~

        2.  Vennerable creates nice Venn diagrams and it's simple to use.
            1.  first: create an empty list vector

                ```
                ref_rpob_pro<-vector(mode="list")
                ```

            2.  second: put groups of ID's want to be compared into the empty vector just created.

                ```
                ref_rpob_pro$ref_proteases_org<-unique(ref_pro)
                ref_rpob_pro$ref_rpoB_org<-unique(ref_rpob.id$X1)
                ```

            3.  third: ask Vennerable to calculate the overlaps.

                ```
                V<-Venn(ref_rpob_pro)
                ```
  
            4. finally: `plot(V)` or save plot directly to file!   

               ~~~
               pdf("ref_rpob_pro_org_venn.pdf")   
               plot(V, doWeight=F)   
               dev.off()   
               ~~~

5. And that's it!

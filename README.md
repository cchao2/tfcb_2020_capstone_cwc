# About the data:

This capstone project uses the dataset from the Glaucoma diagnosis dataset. The questions that this project aims to adress are:

1. Is age related to ocular pressure or cornea thickness? What does that mean for age and glaucoma diagnosis?
2. How does the training dataset compare to the test dataset in terms of diagnosis of glaucoma after training?
3. How well does cornea thickness in this dataset suggest glaucoma diagnoses?

## Measured Variables:

1. Glaucoma: 0 or 1 as not diagnosed or diagnosed with Glaucoma, respectively.
2. Age: age of patients sampled
3. Intraocular pressure (IOP): normal eye pressure ranges from 12-22 mm Hg; eye pressure > 22 mm Hg is considered higher than normal. High eye pressure alone is not indicative of Glaucoma.	
4. Mean deviation (MD): the mean deviation of the PSD?
5. Pattern standard deviation (PSD): provides information about localized loss; measures irregularity by summing absolute value of difference between the threshold value for each point and the average visual field sensitivity at each point. High PSD indicates nonuniform sensitivity loss. 
PSD value may appear to improve as glaucoma advances, due to global depression.
6. Glaucoma Hemifield Test (GHT): compares 24-2 visual fields into 10 regions (5 inferioir regions as mirror images of 5 corresponding superior regions. Differences between superior and inferior zones are compared with normal controls; GHT outside normal limits when at least one zone is at p<1%; 
the borderline is when the p value is between 1%-3%. GHT can help identify early, localized defects. 
7. Cornea thickness: cornea thickness determines how accurate IOP readings are. Thin corneas ( < 555µm ) often show artificially low IOP readings. In this case, patient IOPs may actually be higher than readings show.
8. Retinal nerve fiber layer4.mean (RNFL4.mean): the average value of RNFL SUP, INF, and TMP

In this dataset the investigators state that the PSD, GHT, occular pressure, and age have high values whereas MD and RNFL4.mean have low avlues 
Cornea thickness in the healthy control group is slightly higher than the glaucoma group.

## Motivations and assumptions:

The original manuscript did not provide sufficient information, at least for one who is not familiar with the diagnosis behind glaucoma, to understand what they did with the RNFL4.mean variable measured.
I also could not deduce what the mean deviation (MD) was used for or how it was determined in the first place.  
Since I am not familiar with the diagnosis process behind glaucoma, I did read some papers about these variables measured in this dataset. More specifically, I became interested in looking at the intraocular pressure (ocular pressure)
and cornea thickness. In literature I read, it was stated that cornea thickness affects the ocular pressure measured. Thus I was interested in seeing how these two could vary with age -- if they were affected by age at all.
I was also interested in looking further at whether cornea thickness in pateints with glaucoma were truly lower than the normal controls in both the train and test datasets. 
To examine these questions, I assumed that within the glaucoma columns in the datasets given: 1 = glaucoma and 0 = no glaucoma (control). I also assumed that subjects who were diagnosed with glaucoma in the testing data were diagnosed via the model and were blinded from the researchers;
that way I could compare between the training and testing results. 

## Files:

The repository includes this README.md file, a jupiter notebook, and an R markdown file.
The jupiter notebook contains PCA analysis of the dataset, addressing the first question.
The R markdown file answers the second and third questions. 

## Citations:

Kim, Seongjae; Cho, Kyung Jin; Oh, Sejong (2018), Data from: Development of machine learning models for diagnosis of glaucoma, Dryad, Dataset, https://doi.org/10.5061/dryad.q6ft5

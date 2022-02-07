# MWAS-antid
Config files for my GitHub profile.

#subsetting dispense data to include just SSRIs, linking this to blood draw appointments, and seeing which dispense dates fall within the 12m leading up to the appointment 

> data <- read.csv("1617_0279_PIS.csv")

#subsetting prescription data

> SSRI <- subset(data, PIBNFParagraphCode == "403030")

> table(SSRI["PIApprovedName"])

> glimpse(SSRI)
--Rows: 94,480
---Columns: 29

 #opening linker file

> iddata <- read.xlsx("ST record linkage key.xlsx")

> glimpse(iddata)
Rows: 22,014
Columns: 2

#merging 
> merge <- merge(SSRI, iddata, by.x = "substitutionID")

> dim(merge)
[1] 94480    30

> table(merge$substitutionID)

#getting appt data

> appt <- read.csv("appt.csv")

> glimpse(appt)
Rows: 24,101
Columns: 6

> appt2 <- appt[,c("id","appt")]

#changing date format of appt data 

> appt2 = as.data.frame(appt2)

> appt2_date <- appt2

> appt2_date$appt <- as.Date(appt2$appt, "%d/%m/%Y”)

#changing date format of dispense date data

> merge = as.data.frame(merge)

> mergeSSRI_date <- merge

> mergeSSRI_date$DispDate <- ymd(merge$DispDate)

#merging appt dates with mergeSSRI dates

> appt_dispdates_merge <- merge(mergeSSRI_date, appt2_date, by.x="id")

> dim(appt_dispdates_merge)

#setting up a 6 month interval prior to appt date (as column with dataframe)

> appt_dispdates_merge$intervals <- interval(start=appt_dispdates_merge$appt-months(6), end=appt_dispdates_merge$appt)

#checking which dispense dates are within 6m of the blood draw appt

> dispdates_within_interval = appt_dispdates_merge[which(appt_dispdates_merge$DispDate %within% appt_dispdates_merge$intervals),]

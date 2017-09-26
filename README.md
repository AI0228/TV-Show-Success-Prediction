# What makes a successful TV show?

This analysis and model attemts to determine the factors that influence high or low IMDb ratings for TV shows.  All generes are examined, and while most originate in the United States, there are a few from the UK and elsewhere included.   

Two separate models are developed.  In both, the top and bottom rated shows are classified as winners and losers, respectively, and an array of 12 classisfiers are applied using cross validation to identify the best performing model. 25% of the data was reserved as a test set, and cross-validation scores and test set scores are both shown in the tables below. Baseline score is 0.52.  

The first utilizes natural language processing (NLP) on the IMDb summary descriptions of each show.  Term Frequency - Inverse Document Frequency and Count Vectorization were used on n-grams of size 2-4 were used.  With both vectorization techniqes, Random Forrest and Naive Bayesian classifiers were most successful, with the highest score of 0.642 was achieved using TF-IDF vectorization and a Multinomial Naive Bayes classifier.   The n-grams with highest cumulative score are identified as the most significant factors in the model, giving a clue as to the words in a summary description that foretell a show's likelihood of being a winner or loser.  

A second model, using factors such as genre, lenght, schedule times, network and format was also built, and the same set of 12 classifiers was applied.  The ADA Boost classifier achieved the top score of 0.92 using these factors.  

_Data collection and cleanup was tedious, and involved multiple runs of webscraping IMDb pages on show ratings, then using the TVmaze API to return show detals.  Unless interested in these details, the reader is encouraged to skip to the section titled "Modeling Section" a bit more than halfway through this notebook._

**Results Summary:**

From the NLP models, it seems shows featuring adult characters in crime and drama series set in times before or after the present in New York will fare better than reality or animated series featuring children or teens and highlighting pop culture.  

The model on the factors other than the summary showed similar tendencies.  Realty formats were the strongest negative factor in predicting success, while the scripted format was the strongest positive predictor.  Game and Talk shows were negative, while crime, science fiction, comedy, drama and documentaries were positive predictors.   Shows aired by HBO and BBC predicted success, while the lower rated shows were found more predominantly on MTV, E!, Comedy Central and Lifetime.  

Though interesting associations have been found, it must be said that nothing in the techniques used here can be interpreted as causality. For example, it cannot be said that reality shows featuring teenagers will always flop.  This report is based on initial efforts to determine factors that may influence a show’s success, and have shown a path for future more detailed modeling.  Suggested future paths include textual analysis of critics’ reviews, analysis based on cast or producers, analysis of differences in rating based on audience demographics, and a more detailed look at the connection between genre and the type/style of show.  
  

## Technical Details

**DATA ACQUISITION**

The list of shows used was obtained from IMDb (the Internet Movie Database). A group of 235 highest rated shows was obtained from a list of top TV shows. The IMDb advanced search facility was used to generate a list of bottom rated shows. This proved more difficult, as understandably there is not as much data collected for shows not well received by audiences. IMDb identification numbers were scraped from the displayed web pages, and these were then submitted to the TV Maze API to collect additional information. This included the rating, genre, language, schedule, runtime and network details, as well as a summary description of each show. Considerable effort was required to assemble and clean the dataset, and a final list of 634 shows was narrowed to 491 with complete data. All samples were tagged with an identifier; 1 if from the IMDb top show list (hereafter called winners), and 0 if from the lowest rated show list, hereafter referred to as losers. Ultimately the analysis ready dataset contained 235 winners and 256 losers.

**ANALYSIS & MODEL**

Several approaches were used in the modelling phase.  Summaries were analyzed separately, using both count vectorizer and tf-idf models.   The count vectorizer approach breaks text into segments, in this case 2 to 4 sequential word packages called n-grams, and counts the number of occurrences of each package in each summary.  Tf-idf, or term frequency–inverse document frequency, computes a ratio of the frequency of the n-gram packets in each summary vs. the occurrence in of the same n-gram in all summaries.  This reduces the influence of overuse of certain word patterns by one author with respect to lesser use by the majority of other authors.  These can be thought of as weighting factors, and it is these ratios that are modeled in the tf-idf approach. 

In this study the 2000 most frequent n-grams from each approach were retained and modeled using 12 different classification methods covering logistic regression, discriminant analysis, decision tree, boosting and Bayesian methods.  25% of the samples were held for the final test of the model, and 75% were used for model training.  While the words here are technical, the important point is that each of these models attempts to classify the TV shows as either a winner or loser group based on the words in the summary text.  Models that do better than guessing the most predominant class (in this case loser because 52% of our samples were low rated shows) are considered to be working classifiers, and an accuracy score is calculated for each model that indicates the percentage of times the model correctly predicted the shows classification as a winner or loser.  

Additionally, the remaining data collected was processed, the most important factors determined, and then run through the same modeling process with the 12 classifiers, again reserving 25% of samples to validate the model.  Both approaches, using summary text or the other factors, were successful in predicting into which category a show would fall.




**FINDINGS**

None of the above data and process discussion matters if there are no concrete findings.  In this case, fortunately, indicators of a show success were found, and can be used to suggest other subjects, formats, genres, and networks which seem to give a show an advantage.  

The analysis of summary text resulted in a maximally performing model that showed 70% accuracy.  This used the tf-idf text processing and a Gaussian Naive Bayes classifier.  Of interest is the n-grams (word packages) that showed up in the winner vs. loser categories.

<table class=MsoTableGrid border=1 cellspacing=0 cellpadding=0
 style='margin-left:.5in;border-collapse:collapse;border:none;mso-border-alt:
 solid windowtext .5pt;mso-yfti-tbllook:1184;mso-padding-alt:0in 5.4pt 0in 5.4pt'>
 <tr style='mso-yfti-irow:0;mso-yfti-firstrow:yes'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><o:p>&nbsp;</o:p></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border:solid windowtext 1.0pt;
  border-left:none;mso-border-left-alt:solid windowtext .5pt;mso-border-alt:
  solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Winner<o:p></o:p></b></p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border:solid windowtext 1.0pt;
  border-left:none;mso-border-left-alt:solid windowtext .5pt;mso-border-alt:
  solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Loser<o:p></o:p></b></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:1'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  border-top:none;mso-border-top-alt:solid windowtext .5pt;mso-border-alt:solid windowtext .5pt;
  padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Location<o:p></o:p></b></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal>New York</p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal>Los Angeles</p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:2'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  border-top:none;mso-border-top-alt:solid windowtext .5pt;mso-border-alt:solid windowtext .5pt;
  padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Type<o:p></o:p></b></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal>Documentary</p>
  <p class=MsoNormal>Crime</p>
  <p class=MsoNormal>Drama</p>
  <p class=MsoNormal>Series based</p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal>Reality </p>
  <p class=MsoNormal>Animated</p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:3'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  border-top:none;mso-border-top-alt:solid windowtext .5pt;mso-border-alt:solid windowtext .5pt;
  padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Writer / Producer <o:p></o:p></b></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal>David Attenborough</p>
  <p class=MsoNormal>Andrew Davies</p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt'>
  <p class=MsoNormal><o:p>&nbsp;</o:p></p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:4;height:26.05pt'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  border-top:none;mso-border-top-alt:solid windowtext .5pt;mso-border-alt:solid windowtext .5pt;
  padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Characters /
  Setting<o:p></o:p></b></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal>Follows brothers</p>
  <p class=MsoNormal>Men women</p>
  <p class=MsoNormal>Main character</p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal>Children</p>
  <p class=MsoNormal>High School</p>
  <p class=MsoNormal>Group Teenagers</p>
  <p class=MsoNormal>Real Housewives</p>
  <p class=MsoNormal>Pop culture</p>
  </td>
 </tr>
 <tr style='mso-yfti-irow:5;mso-yfti-lastrow:yes;height:26.05pt'>
  <td width=121 valign=top style='width:121.25pt;border:solid windowtext 1.0pt;
  border-top:none;mso-border-top-alt:solid windowtext .5pt;mso-border-alt:solid windowtext .5pt;
  padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal><b style='mso-bidi-font-weight:normal'>Timing<o:p></o:p></b></p>
  </td>
  <td width=135 valign=top style='width:135.0pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal>Years later</p>
  <p class=MsoNormal>Years before</p>
  </td>
  <td width=122 valign=top style='width:121.5pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  mso-border-top-alt:solid windowtext .5pt;mso-border-left-alt:solid windowtext .5pt;
  mso-border-alt:solid windowtext .5pt;padding:0in 5.4pt 0in 5.4pt;height:26.05pt'>
  <p class=MsoNormal><o:p>&nbsp;</o:p></p>
  </td>
 </tr>
</table>
</br>
</br>

From this we might deduce that shows featuring adult characters in crime and drama series set in times before or after the present in New York will fare better than reality or animated series featuring children or teens and highlighting pop culture. 

The model on the factors other than the summary showed similar tendencies.  Realty formats were the strongest negative factor in predicting success, while the scripted format was the strongest positive predictor.  Game and Talk shows were negative, while crime, science fiction, comedy, drama and documentaries were positive predictors.   Shows aired by HBO and BBC predicted success, while the lower rated shows were found more predominantly on MTV, E!, Comedy Central and Lifetime.

Though interesting associations have been found, it must be said that nothing in the techniques used here can be interpreted as causality. For example, it cannot be said that reality shows featuring teenagers will always flop.  This report is based on initial efforts to determine factors that may influence a show’s success, and have shown a path for future more detailed modeling.  Suggested future paths include textual analysis of critics’ reviews, analysis based on cast or producers, analysis of differences in rating based on audience demographics, and a more detailed look at the connection between genre and the type/style of show.




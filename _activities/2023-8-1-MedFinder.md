---
title: "Med Finder (3rd Place Winner of Husky Startup Challenge)"
collection: activities
type: startup
permalink: /activities/med-finder/ 
role: Engineering Lead
period: Aug 2023 - Present
authors: 
bookcover: 
classes: wide
description: <p><ul><li>Performed polynomial regression to predict medicine inventory data accurately by iterating different polynomial orders and selecting the best RMSE.</li><li>Created random medicine inventory data with different probability distributions that reflected actual inventory data to perform polynomial regression.</li><li>Developed medicine search feature on Med Finder website that displayed a list of pharmacies based on medicine availabilities and distances by querying inventory database and Mapbox API for geocoding data.</li></ul></p>
---
<a style="text-decoration: none;" href="https://www.fastermedsolutions.com/" target="_blank">Med Finder <i class="fa fa-external-link-alt"></i></a><br />
<a style="text-decoration: none;" href="/assets/activities/med-finder/med-finder-pitch-deck.pdf" target="_blank">IR Deck <i class="fa fa-file"></i></a><br />
<!-- <a style="text-decoration: none;" href="https://github.com/tae-h-yang/med-finder" target="_blank">Code <i class="fa fa-code"></i></a><br /> -->

# Introduction
Med Finder provides an online medicine search platform to patients who are experiencing drug shortage. It checks medicine availability based on pharmacies' inventory data so that the patients can find pharmacies that have their medicine. Furthermore, the inventory data are analyzed using Machine Learning and 100% accurate medicine availablity information can be offered to the patients. Our performance at Husky Startup Challenge can be found <a style="text-decoration: none;" href="/activities/entrepreneurs-club/">here.</a>

# Inventory Data Generation
<p style="text-align: center;"><img src="/assets/activities/med-finder/inventory-data-generated.png" width="800" height="800" /><strong><br />Fig. 1: Medicine inventory data generated.</strong></p>
Med Finder has partnered with 4 individual pharmacies so far and they are willing to disclose their inventory data for our service. Before analyzing their data, test inventory data were generated as in Fig. 1 to validate our inventory prediction model. The test data was designed to show decreasing medicine quantities and restocking when reaching below certain threshold values. Different Gaussian distributions were used for daily sales, restock amounts and thresholds. Integrity of the data were verified by our Pharmacy student. Fig. 2 below shows an example of medicine quantities from the test inventory data.
<p style="text-align: center;"><img src="/assets/activities/med-finder/quantity-over-time.png" width="400" height="400" /><strong><br />Fig. 2: Example of medicine quantities from test inventory data over time.</strong></p>

# Inventory Data Prediction
<p style="text-align: center;"><img src="/assets/activities/med-finder/quantity-prediction.png" width="400" height="400" /><strong><br />Fig. 3: Example of medicine quantity prediction.</strong></p>
Fig. 3 shows an example of medicine quantity prediction that utilized polynomial regression on the test inventory data. Several polynomial orders were iterated and the optimal order with the best RMSE was determined. The red trendline predicted when restocking would occur by its local minimum. Although there were some errors with actual medicine quantities, the prediction model was accurate enough to provide information about medicine availability. Since Med Finder aimed to help patients to find pharmacies with their medicine, it was important to filter out pharmacies that were likely not to have their medicine and the prediction model was validated to perform its task.

The restocking information from the prediction model can be sent back to pharmacies to optimize their inventory and reduce costs of storing medicine in stock. Furthermore, prediction models with actual inventory data of several years can be analyed in the future to provide more information such as when seasonal flu or pandemic would happen. This could be performed by classifying inventory data during the flu or pandemic and comparing with the prediction model. Obvisouly, overfitting problems are expected when analyzing actual inventory data. These would be resolved by utilizing regularization techniques, cross-validation, ensemble methods, etc.

# Medicine Search Feature
<p style="text-align: center;"><img src="/assets/activities/med-finder/medicine-search.png" width="400" height="400" /><strong><br />Fig. 4: Medicine search page on Med Finder.</strong></p>

As a minimum viable product, medicine search feature was developed on <a style="text-decoration: none;" href="https://www.fastermedsolutions.com/" target="_blank">Med Finder website.</a> The search page of the feature received a medicine name and zip code as shown in Fig. 4. Its backend searched through pharmacy inventory database, queried pharmacy information based on the medicine availability, and calculated distances between the user's zip code and pharmacies using Mapbox API for geocoding data. Lastly, a list of pharmacies that had the searched medicine was displayed on the result page as shown in Fig. 5.

<p style="text-align: center;"><img src="/assets/activities/med-finder/medicine-result.png" width="400" height="400" /><strong><br />Fig. 5: Medicine result page on Med Finder.</strong></p>
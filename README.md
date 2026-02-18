# AIML-Project2
The goal of this project is to use CRISP-DM framework to understand what factors determine the price of a car based on the provided data set and communicate the results as actionable items to a car dealership

The link to Dataset [Link to Dataset](/data/vehicles.csv)

The link to Jupyter Notebook can be found here: [Link to Jupyter Notebook](/What_drives_the_price_of_a_car.ipynb)

# Business Understanding
Car dealerships need to increase their profitability by improving their inventory and maximizing the sale price of the vehicles. The higher the churn rate of their inventory the better their revenue will be leading to better profitability.To do this the dealership needs to know the optimal price at which a vehicle can be sold without the vehicle being in their inventory for a long duration.
So it is important for the dealership to identify what factors have the highest impact on the price of a vehicle and figure out an optimal price for a given vehicle.

We will look at past sale data of various vehicles and analyze all the features which contributed to it's sale price using a regression model and make predictions on what could be an optimal price for a given vehicle.

# Summary of Data

**Dataset Size & Quality:**
- Starting dataset: 426,880 cars with 17 features
- After cleaning: 267,673 cars
- Data quality improvement: Removed outliers and invalid entries (cars with $0 price, odometers >500K, unrealistic years)

**Price Distribution (Target Variable):**
- Mean price: $17,059
- Range: $1,000 to $54477
- Most cars cluster in the $5K-$25K range (this matches typical used car market)

# Modeling

I ran multiple different regressions and compared their MSE and R² score to find the best model:

| Model | Train MSE | Test MSE | Train R² | Test R² |
|-------|-----------|----------|----------|---------|
| Linear Regression | 3.60e7 | 3.57e7 | 0.7498 | 0.7534 |
| **Linear Regression with Pipeline** | **3.40e7** | **3.48e7** | **0.7637** | **0.7589** |
| Ridge Regression with Pipeline | 3.43e7 | 3.51e7 | 0.7621 | 0.7571 |
| Lasso Regression with Pipeline | 3.60e7 | 3.68e7 | 0.7506 | 0.7453 |

**Best Model:** Linear Regression with Pipeline (highlighted) - Test R² of 0.7589 indicates it explains 75.8% of price variation	

# Features analysis

I performed a Permutation importance analysis on the Linear Regression with Pipeline model to identify the top 15 features that impact car prices:

| Rank | Feature | Importance | Std Dev | Impact Level |
|------|---------|------------|---------|--------------|
| 1 | Year | 0.4002 | 0.0016 | **Very High** |
| 2 | Model | 0.2264 | 0.0013 | **High** |
| 3 | Odometer | 0.0967 | 0.0008 | **Moderate** |
| 4 | Fuel | 0.0232 | 0.0004 | Low |
| 5 | Transmission | 0.0193 | 0.0004 | Low |
| 6 | Drive | 0.0173 | 0.0003 | Low |
| 7 | Cylinders | 0.0123 | 0.0002 | Low |
| 8 | Type | 0.0117 | 0.0003 | Low |
| 9 | Title Status | 0.0072 | 0.0002 | Very Low |
| 10 | State | 0.0053 | 0.0001 | Very Low |
| 11 | Manufacturer | 0.0053 | 0.0001 | Very Low |
| 12 | Condition | 0.0003 | 0.0001 | Negligible |
| 13 | Paint Color | 0.0002 | 0.0001 | Negligible |
| 14 | Size | 0.0001 | 0.0000 | Negligible |

**Key Insight:** Year, Model, and Odometer account for **72.5% of price prediction power** — focusing on these three factors alone captures most of the value

# Final summary and next steps

*The #1 Price Driver: Vehicle Age (40% impact)**
- **Finding:** The year a car was made is by FAR the most important factor in its price
- **What this means:** A 2020 Honda vs a 2010 Honda (same model, condition, mileage) can differ by $5,000-$10,000+
- **Action item:** Prioritize acquiring newer vehicle models (2015+) when possible—they hold value better and command higher prices naturally

**The #2 Price Driver: Specific Car Model (22.7% impact)**
- **Finding:** The specific model name matters almost as much as the year
- **What this means:** A Nissan Altima 2018 vs Toyota Camry 2018 will have very different prices due to brand reputation, reliability, and market demand
- **Action item:** Focus your inventory on high-demand models in your market (research local sales data). Avoid slow-moving brands.

**The #3 Price Driver: Odometer Reading (9.7% impact)**
- **Finding:** Mileage does matter, but not as much as age and model
- **What this means:** A car with 200K miles is significant cheaper than 100K miles, BUT an older car with low miles is still worth less than a newer car with high miles
- **Real example:** 2020 with 150K miles < 2018 with 50K miles < 2022 with 80K miles (in price order)
- **Action item:** Don't overpay for low-mileage old cars. Focus on newer cars with moderate mileage as your sweet spot for margin

**Minor Price Drivers (4% or less impact each):**
- Fuel type (diesel/electric get ~10% premium)
- Transmission (manual vs automatic)
- Number of cylinders
- Vehicle condition
- Paint color

### NEXT STEPS

**1. Validate Model Predictions**
- Test the pricing model on your current inventory
- Compare model-suggested prices to your actual listed prices
- Check if cars priced higher than model prediction are slow sellers
- Check if cars priced lower than model prediction are selling fast

**2. Audit Your Acquisition Strategy**
- Analyze your current inventory: What % are 2015+ models? What % have odometer >150K?
- Compare your average acquisition cost vs model prediction for cars you've bought
- Identify if you're overpaying for certain categories (e.g., old trucks with low miles)

**3. Review Existing Pricing**
- Pick 10-15 cars currently on your lot and run them through the model. If suggested price is 5-10% different, consider repricing document which cars moved faster after repricing

**4. Implement Dynamic Pricing**
- Start using the model to price new acquisitions within 24 hours of purchase
- Train sales staff on why a car is priced at $X (confidence in pricing = better sales conversations)

**5. Optimize Sourcing**
- Ensure that all new cars in the inventory follow the below guidelines
  - 60% of inventory should be 2016+ models
  - Average odometer should be <120K miles
  - Focus on top 5-10 models that sell quickly in your area
  
**6. Monitor Price Accuracy**
- Create a dashboard tracking:
  - Days-on-lot for model-priced vs non-model-priced cars
  - Actual selling price vs model-predicted price
  - Profit margin by car age/model

**7. Improve Model by getting more data**
- Collect more data on:
  - Vehicle accident history
  - Service history
  - Interior/exterior condition grades (photos)
  - Local market demand by season
  - Competitor pricing in your area
  
- Retrain model with additional variables to improve from 75.7% to 80%+ accuracy

**8. Create Predictive Inventory Tool**
- Build interface where staff can input: Year, Model, Mileage, Condition
- System returns: Recommended price ± margin
- Staff can accept recommendation or override with justification
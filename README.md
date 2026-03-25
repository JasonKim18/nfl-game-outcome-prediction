# NFL Game Outcome Prediction (2018–2023)  
**Jason Kim**

## Overview
In this project, I try to predict whether the home team wins an NFL game using publicly available pre-game information.

The main questions I look at are:
1. How strong are simple baselines like:
   - always picking the home team  
   - always picking the betting favorite?  
2. Can I build a non-market model that only uses team performance and basic game information?  
3. If I combine team form with Vegas betting lines, can I build a model that:
   - slightly beats the betting favorite in accuracy, and  
   - gives better probability estimates (measured by ROC AUC)?  

I use regular-season NFL games from 2018–2023 and train logistic regression models to compare these approaches.

---

## Dataset
- Source: `nfl_data_py`  
- Seasons: 2018–2023 (regular season only)  
- Total games: 1,583  

Some key features used:
- Rest days (`home_rest`, `away_rest`)  
- Divisional games (`div_game`)  
- Weather (`temp`, `wind`)  
- Betting data (moneyline, spread)  

Missing weather values were handled by:
- Setting dome games to ~70°F and 0 wind  
- Filling remaining values with the median  

---

## Target Variable
The goal is to predict whether the home team wins:

- `1` → home team wins  
- `0` → home team loses (or ties)  

Overall home win rate: ~53.9%

---

## Modeling Approach

### Train/Test Split
- Train: 2018–2022 seasons  
- Test: 2023 season  

### Baselines
I start with two simple baselines:
- Home team baseline → ~55.5% accuracy  
- Betting favorite baseline → ~68.4% accuracy  

This shows right away that betting markets are already very strong.

---

## Models

### 1. Basic Non-Market Model
Uses only:
- Rest  
- Division  
- Weather  

Results:
- Accuracy: ~54.4%  
- ROC AUC: ~0.51  

This performs poorly and doesn’t beat the home baseline.

---

### 2. Non-Market Model (with Team Form)
Adds team performance features like:
- Last 5 game win %  
- Point differential  
- Rolling averages  

Results:
- Accuracy: ~58.1%  
- ROC AUC: ~0.64  

This shows team form adds real signal, but still isn’t enough to match betting markets.

---

### 3. Market-Only Model
Uses only:
- Moneyline  
- Spread  

Results:
- Accuracy: ~68.0%  
- ROC AUC: ~0.69  

This matches the betting favorite baseline very closely.

---

### 4. Market + Basic Features
Adds:
- Rest  
- Weather  
- Division  

Results:
- Accuracy: ~68.4%  
- ROC AUC: ~0.695  

Very little improvement, suggesting this info is already priced into betting lines.

---

### 5. Final Combined Model
Combines:
- Market data  
- Basic features  
- Team form features  

Results:
- Accuracy: ~68.75%  
- ROC AUC: ~0.69  

This is the best overall model.

---

## Key Results Summary

| Model | Accuracy | ROC AUC |
|------|--------|--------|
| Home Team Baseline | ~55.5% | 0.50 |
| Betting Favorite | ~68.4% | — |
| Non-Market (Basic) | ~54.4% | ~0.51 |
| Non-Market + Team Form | ~58.1% | ~0.64 |
| Market Only | ~68.0% | ~0.69 |
| Final Combined Model | **~68.75%** | **~0.69** |

---

## Interpretation
The betting favorite baseline is already very strong, which makes this problem difficult.

The final model only slightly improves accuracy, but the ROC AUC shows that it does a better job ranking games by how likely the home team is to win.

This suggests that team-form features help more with probability quality than just predicting wins and losses.

---

## Key Takeaways
- Betting markets are extremely efficient and hard to beat  
- Team performance features do add signal, but not enough on their own  
- Most gains show up in probability quality (ROC AUC), not raw accuracy  
- Combining features gives small but consistent improvements  

---

## Tech Stack
- Python  
- pandas  
- numpy  
- scikit-learn  
- matplotlib  
- seaborn  
- Jupyter Notebook  

---

## Future Work
If I extended this project, I would:
- Add player-level data (QB stats, injuries, etc.)  
- Track betting line movement over time  
- Try more flexible models like gradient boosting  
- Improve feature engineering  

---

## Notes
This project showed how hard real-world prediction problems can be, especially when competing against something like betting markets. Even small improvements required careful feature engineering and model comparison.

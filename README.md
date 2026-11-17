# Carbon Coach

A FastAPI backend that tracks your daily carbon footprint from transport and food, compares it to a daily target, and gives you specific swaps to bring it down.

## What it does

- **Log activity:** trips (car, bus, subway, bike, walk) by distance, and meals (beef, pork, chicken, and more) by weight
- **Calculate emissions** with per-mode and per-food emission factors. For car trips you can also give make, model, engine type (petrol, diesel, hybrid, EV) and year for a vehicle-specific estimate (`data/emission_factors.py`)
- **Daily target:** 8 kg CO2
- **Summarize** your footprint by day, week, month or year
- **Suggest improvements** that are tied to what you actually logged. For example: "Swapping one beef meal for chicken saves around 20 kg CO2."

## API

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/log/trip` | Log a trip (`mode`, `distance_km`, `city`, optional car details) |
| POST | `/log/food` | Log a meal (`food_type`, `weight_grams`) |
| GET | `/summary/today` | Today's total vs. the daily target |
| GET | `/summary/week` · `/month` · `/year` | Rolling summaries |
| GET | `/suggestions` | Personalized reduction tips |

All endpoints take a `user_id`. Inputs are validated with Pydantic, so negative distances or weights are rejected.

## Run it

```bash
pip install -r requirements.txt
uvicorn main:app --reload
```

Then open http://127.0.0.1:8000/docs for the interactive Swagger UI.

## Structure

```
api/       FastAPI routes + request validation
engine/    emission calculator and suggestion logic
data/      emission factors and daily target
db/        SQLite setup and queries
models/    trip/food entry models and enums
main.py    app entry point
```

## Stack

Python · FastAPI · Pydantic · SQLite · Uvicorn

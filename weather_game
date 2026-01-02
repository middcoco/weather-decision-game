import streamlit as st
import random
import pandas as pd

# -----------------------------
# Initialize session state
# -----------------------------
if "round" not in st.session_state:
    st.session_state.round = 1
if "score" not in st.session_state:
    st.session_state.score = 0
if "history" not in st.session_state:
    st.session_state.history = []

# -----------------------------
# Title and instructions
# -----------------------------
st.title("🌦️ The Weather Decision Game")
st.write("""
Welcome! In this game, you have to **decide whether to bring an umbrella** based on the weather forecast.  

Remember:
- Forecasts are **probabilities**, not certainties.
- Sometimes making the "right choice" still leads to bad outcomes.
- The goal is to **minimize your regrets** over multiple days.
""")

# -----------------------------
# Simulate a weather forecast
# -----------------------------
# Forecast probability between 30% and 90%
forecast_prob = random.choice([30, 40, 50, 60, 70, 80, 90])
st.subheader(f"Day {st.session_state.round}")
st.write(f"The weather forecast says there is a **{forecast_prob}% chance of rain** today.")

# -----------------------------
# Player decision
# -----------------------------
col1, col2 = st.columns(2)
with col1:
    bring_umbrella = st.button("Bring Umbrella ☂️")
with col2:
    no_umbrella = st.button("Don't Bring Umbrella 🌞")

# -----------------------------
# Resolve outcome
# -----------------------------
if bring_umbrella or no_umbrella:
    # Randomly sample whether it rains
    did_rain = random.random() < forecast_prob / 100

    # Scoring rules
    if bring_umbrella:
        if did_rain:
            outcome_text = "You stayed dry! ✅"
            score_change = 2
        else:
            outcome_text = "You carried an umbrella for no reason. 😅"
            score_change = -1
    else:
        if did_rain:
            outcome_text = "Oh no! You got wet! 🌧️"
            score_change = -2
        else:
            outcome_text = "Good call! No umbrella needed. 😎"
            score_change = 1

    # Update session state
    st.session_state.score += score_change
    st.session_state.history.append({
        "Day": st.session_state.round,
        "Forecast": f"{forecast_prob}%",
        "Rained": did_rain,
        "Decision": "Umbrella" if bring_umbrella else "No Umbrella",
        "Outcome": outcome_text,
        "Score Change": score_change,
        "Total Score": st.session_state.score
    })
    st.session_state.round += 1

    # Display outcome
    st.info(outcome_text)
    st.write(f"Current Score: {st.session_state.score}")

# -----------------------------
# Show history and charts
# -----------------------------
if st.session_state.history:
    st.subheader("Game History")
    df = pd.DataFrame(st.session_state.history)
    st.dataframe(df)

    # Chart: Score over days
    st.line_chart(df.set_index("Day")["Total Score"])

# -----------------------------
# Ending message
# -----------------------------
if st.session_state.round > 10:
    st.success("""
Game Over! 🎉  
Remember: probability is not certainty. Even good choices sometimes lead to bad outcomes, and bad choices sometimes pay off.  
Try again to see how your decisions stack up!
""")
    # Reset button
    if st.button("Restart Game"):
        st.session_state.round = 1
        st.session_state.score = 0
        st.session_state.history = []
        st.experimental_rerun()

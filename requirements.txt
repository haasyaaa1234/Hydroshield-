import streamlit as st
import time

st.set_page_config(
    page_title="HydroShield - Smart Drainage Monitor",
    page_icon="🌊",
    layout="centered"
)

st.title("🌊 HydroShield Live Dashboard")
st.markdown("**IoT-Powered Urban Drainage & Waste Prevention System**")
st.markdown("---")

water_height_cm = 12.0  
flow_rate_lpm = 14.5    
rain_status = "Dry"     

is_clogged = (flow_rate_lpm < 2.0 and water_height_cm > 15.0)
is_overflow = (water_height_cm > 22.0)

col1, col2, col3 = st.columns(3)

with col1:
    st.metric(label="Water Depth", value=f"{water_height_cm} cm", delta="Overflow Risk" if is_overflow else "Normal Level")

with col2:
    st.metric(label="Flow Speed", value=f"{flow_rate_lpm} L/min", delta="Blockage Drop" if is_clogged else "Stable Flow")

with col3:
    st.metric(label="Weather State", value=rain_status, delta="Active Node")

st.markdown("---")
st.subheader("🚦 System Health & Diagnostics")

if is_clogged:
    st.error("🚨 **RED ALERT: BLOCKAGE DETECTED NEAR DRAINAGE!**")
    st.markdown("""
    * **Diagnosis:** Plastic material or foreign debris is stuck near the drainage grate.
    * **Automated Actions:** Hardware buzzer active, SMS text alert dispatched.
    """)
    st.audio("https://www.soundjay.com/buttons/beep-01a.mp3", format="audio/mp3", autoplay=True)
elif is_overflow:
    st.warning("⚠️ **YELLOW WARNING: High Water Runoff / Accumulation.**")
else:
    st.success("🟢 **GREEN STATUS: Normal Flow.** Drainage is clear.")

st.markdown("---")
st.subheader("🛠️ Exhibition Demonstration Panel")

if 'demo_clog' not in st.session_state:
    st.session_state.demo_clog = False

col_btn1, col_btn2 = st.columns(2)
with col_btn1:
    if st.button("🧪 Trigger Test Blockage (Simulate Plastic Clog)"):
        st.session_state.demo_clog = True
        st.toast("Simulating emergency block... SMS and Alarms firing!", icon="🚨")
        st.rerun()

with col_btn2:
    if st.button("🔄 Reset to Normal Flow"):
        st.session_state.demo_clog = False
        st.toast("System reset to normal status.", icon="🟢")
        st.rerun()

if st.session_state.demo_clog:
    water_height_cm = 24.5
    flow_rate_lpm = 0.5
    is_clogged = True

if st.checkbox("Enable Real-Time Sensor Sync (Auto-refresh every 3s)", value=True):
    time.sleep(3)
    st.rerun()

# BlazeDemo-E2E-Performance-Testing-Project-using-Jmeter

Project Overview
This project is an end-to-end performance testing framework built using Apache JMeter for the BlazeDemo flight booking website.
The goal of this project is to simulate real-user behavior for booking flights and to measure:
1. Response times
2. Throughput
3. Error rates
4. End-to-end transaction performance

Website under test: https://blazedemo.com

🎯 Objectives

Validate performance of critical user journey (flight booking)
Measure total transaction time using Transaction Controller
Perform load testing with multiple concurrent users
Apply assertions to validate functional correctness
Use parameterization and correlation for realistic testing

🧱 Test Plan Structure
1. Test Plan
BlazeDemo_E2E_Performance_Test
Central container for all test elements

2. User Defined Variables
Variable	Description
BASE_URL	Application base URL
PROTOCOL	HTTP protocol (https)
THINK_TIME	User think time between actions

3. Thread Group: E2E_User_Load
Setting	Value
Threads (Users)	50
Ramp-Up Period	10 seconds
Loop Count	5
Simulates concurrent users performing booking operations.

4. Config Elements
HTTP Request Defaults
Protocol: https
Server Name: blazedemo.com
HTTP Header Manager
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0
HTTP Cookie Manager
Maintains session consistency
CSV Data Set Config
File: cities.csv
Variables: from, to
Used for source and destination city parameterization

5. Logic Controller
Transaction Controller: Flight_Booking_Transaction
Measures total response time of the complete booking flow
Includes timers and post-processors

6. HTTP Samplers
🏠 Home_Page => Method: GET, Path: /, Assertion: Verifies homepage load
🔍 Search_Flights => Method: POST, Path: /reserve.php, Parameters: fromPort, toPort
Extracts dynamic flight value using Regex Extractor
✈️ Choose_Flight => Method: POST, Path: /purchase.php
Uses correlated flight parameter
💳 Purchase_Ticket => Method: POST, Path: /confirmation.php
Submits passenger and payment details

7. Assertions
Response Assertions for functional validation
Duration Assertion (Global): Max response time 3000 ms

8. Timers
Constant Timer: Think time between requests
Gaussian Random Timer (optional) for realistic load

9. Listeners
Listener	Purpose
View Results Tree	Debugging only
Summary Report	High-level metrics
Aggregate Report	Performance analysis
Graph Results	Visualization

10. tearDown Thread Group (Optional)
Used for cleanup or logout steps after execution

📊 Key Metrics Captured
Average response time
90th & 95th percentile
Error percentage
Throughput
End-to-end transaction time

🧪 How to Execute
GUI Mode (Debug) => Open JMeter => Load Test Plan.jmx
Run with 1 user

Non-GUI Mode (Performance) : Headless
jmeter -n -t blazedemo_e2e_performance.jmx -l results.jtl -e -o report

# Bio-AI
import React, { useState } from 'react';

// Main App component
const App = () => {
    const [activeTab, setActiveTab] = useState('biologicalInputs'); // State for active tab

    const renderTabContent = () => {
        switch (activeTab) {
            case 'biologicalInputs':
                return <BiologicalInputsAndPrediction />;
            case 'goalsProtocols':
                return <GoalsAndProtocols />;
            default:
                return <BiologicalInputsAndPrediction />;
        }
    };

    const tabClasses = (tabName) =>
        `py-3 px-6 text-lg font-semibold rounded-t-lg transition-all duration-200 ${
            activeTab === tabName
                ? 'bg-blue-600 text-white shadow-lg'
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
        }`;

    return (
        <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 flex items-center justify-center p-4 sm:p-6 font-inter antialiased">
            <div className="bg-white p-6 md:p-8 rounded-2xl shadow-2xl w-full max-w-4xl border border-gray-200">
                <h1 className="text-3xl md:text-4xl font-extrabold text-gray-900 mb-6 text-center">
                    Predictive Bio AI <span className="text-blue-600">(Simulation App)</span>
                </h1>
                <p className="text-gray-600 text-center mb-8">
                    Explore simulated biological risks, get AI-driven insights, and manage your longevity protocols.
                    <br /><span className="font-semibold text-red-500">This is a demonstration; not real medical advice.</span>
                </p>

                {/* Tab Navigation */}
                <div className="flex justify-center mb-8">
                    <button onClick={() => setActiveTab('biologicalInputs')} className={tabClasses('biologicalInputs')}>
                        Biological Inputs & Prediction
                    </button>
                    <button onClick={() => setActiveTab('goalsProtocols')} className={tabClasses('goalsProtocols')}>
                        Goals & Protocols
                    </button>
                </div>

                {/* Render Active Tab Content */}
                {renderTabContent()}
            </div>
        </div>
    );
};

// Component for Biological Inputs and Prediction
const BiologicalInputsAndPrediction = () => {
    // --- State Variables for User Inputs ---
    const [age, setAge] = useState(45);
    const [lifestyleScore, setLifestyleScore] = useState(6); // 1-10, 1=poor, 10=excellent
    const [geneticMarker, setGeneticMarker] = useState('none'); // 'none', 'APOE_e4', 'FOXO3_variant'
    const [bloodPressure, setBloodPressure] = useState(120); // Systolic BP
    const [cholesterol, setCholesterol] = useState(180); // Total Cholesterol
    const [sleepHours, setSleepHours] = useState(7); // Hours of sleep per night
    const [cReactiveProtein, setCReactiveProtein] = useState(1.5); // mg/L
    const [microbiomeDiversity, setMicrobiomeDiversity] = useState(7); // 1-10, 1=low, 10=high
    const [telomereLength, setTelomereLength] = useState('average'); // 'short', 'average', 'long'

    // --- State Variables for AI Output and UI State ---
    const [predictionResult, setPredictionResult] = useState(null);
    const [aiExplanation, setAiExplanation] = useState('');
    const [loadingPrediction, setLoadingPrediction] = useState(false);
    const [loadingExplanation, setLoadingExplanation] = useState(false);
    const [error, setError] = useState('');

    // --- Simulated AI Prediction Logic ---
    const runPrediction = async () => {
        setLoadingPrediction(true);
        setPredictionResult(null);
        setAiExplanation('');
        setError('');

        // Simulate AI processing delay
        await new Promise(resolve => setTimeout(resolve, 2000));

        let cardioRiskScore = 0;
        let metabolicRiskScore = 0;
        let cognitiveRiskScore = 0;
        let recommendations = new Set(); // Use a Set to avoid duplicate recommendations

        // Age
        if (age >= 70) {
            cardioRiskScore += 3; metabolicRiskScore += 2; cognitiveRiskScore += 3;
            recommendations.add("Consider comprehensive geriatric assessment and regular screenings for age-related conditions.");
        } else if (age >= 50) {
            cardioRiskScore += 2; metabolicRiskScore += 1; cognitiveRiskScore += 2;
            recommendations.add("Focus on maintaining a balanced diet, regular exercise, and preventative health screenings.");
        } else if (age >= 30) {
            cardioRiskScore += 1; metabolicRiskScore += 0; cognitiveRiskScore += 1;
            recommendations.add("Build strong foundational health habits for long-term well-being.");
        }

        // Lifestyle Score
        if (lifestyleScore <= 3) {
            cardioRiskScore += 3; metabolicRiskScore += 3; cognitiveRiskScore += 3;
            recommendations.add("Urgent review of diet, exercise, and stress management is highly recommended.");
        } else if (lifestyleScore <= 6) {
            cardioRiskScore += 2; metabolicRiskScore += 2; cognitiveRiskScore += 2;
            recommendations.add("Implement more consistent exercise, balance your diet, and reduce sedentary time.");
        } else {
            recommendations.add("Excellent lifestyle habits. Continue current routines and explore further optimization.");
        }

        // Genetic Markers
        if (geneticMarker === 'APOE_e4') { // Simulated marker for increased cognitive/cardio risk
            cardioRiskScore += 1; cognitiveRiskScore += 3;
            recommendations.add("Consult a genetic counselor to understand the implications of APOE e4. Focus on brain-protective diets and cognitive engagement.");
        } else if (geneticMarker === 'FOXO3_variant') { // Simulated marker for protective effect
            cardioRiskScore -= 1; metabolicRiskScore -= 1; cognitiveRiskScore -= 1;
            recommendations.add("FOXO3 variant may confer longevity advantages; continue monitoring and healthy lifestyle.");
        }

        // Blood Pressure
        if (bloodPressure >= 140) {
            cardioRiskScore += 3;
            recommendations.add("Seek medical advice for managing high blood pressure. Dietary changes (low sodium) and regular exercise are crucial.");
        } else if (bloodPressure >= 130) {
            cardioRiskScore += 2;
            recommendations.add("Monitor blood pressure regularly. Consider lifestyle changes (e.g., DASH diet) to lower it.");
        }

        // Cholesterol
        if (cholesterol >= 240) {
            cardioRiskScore += 3; metabolicRiskScore += 1;
            recommendations.add("Immediate medical consultation for high cholesterol. Dietary interventions (reducing saturated/trans fats) are essential.");
        } else if (cholesterol >= 200) {
            cardioRiskScore += 2;
            recommendations.add("Work on lowering cholesterol through diet (more fiber) and increased physical activity.");
        }

        // Sleep Hours
        if (sleepHours < 6) {
            cardioRiskScore += 1; cognitiveRiskScore += 1; metabolicRiskScore += 1;
            recommendations.add("Prioritize consistent sleep of 7-9 hours per night; it significantly impacts overall health.");
        } else if (sleepHours > 9) {
            recommendations.add("While sufficient sleep is good, consistently more than 9 hours might warrant investigation for underlying issues.");
        }

        // C-Reactive Protein (Inflammation Marker)
        if (cReactiveProtein >= 5) {
            cardioRiskScore += 2; metabolicRiskScore += 2;
            recommendations.add("Address high inflammation levels. Consider anti-inflammatory diet and consult a doctor.");
        } else if (cReactiveProtein >= 3) {
            cardioRiskScore += 1; metabolicRiskScore += 1;
            recommendations.add("Monitor inflammation. Focus on stress reduction and healthy diet to keep CRP in check.");
        }

        // Microbiome Diversity
        if (microbiomeDiversity <= 3) {
            metabolicRiskScore += 2; cognitiveRiskScore += 1;
            recommendations.add("Focus on gut health. Increase fiber intake and consume fermented foods to boost microbiome diversity.");
        } else if (microbiomeDiversity <= 6) {
            metabolicRiskScore += 1;
            recommendations.add("Maintain good gut health practices to optimize microbiome diversity.");
        }

        // Telomere Length
        if (telomereLength === 'short') {
            cardioRiskScore += 2; cognitiveRiskScore += 2; metabolicRiskScore += 2;
            recommendations.add("Consider lifestyle factors known to impact telomere health: regular exercise, stress management, and nutrient-rich diet.");
        }


        // Determine overall risk categories
        const getRiskLevel = (score) => {
            if (score <= 3) return { level: "Low Risk", color: "text-green-600" };
            if (score <= 6) return { level: "Moderate Risk", color: "text-orange-600" };
            return { level: "High Risk", color: "text-red-600" };
        };

        const cardioRisk = getRiskLevel(cardioRiskScore);
        const metabolicRisk = getRiskLevel(metabolicRiskScore);
        const cognitiveRisk = getRiskLevel(cognitiveRiskScore);

        setPredictionResult({
            cardio: cardioRisk,
            metabolic: metabolicRisk,
            cognitive: cognitiveRisk,
            recommendations: Array.from(recommendations), // Convert Set back to Array
        });

        setLoadingPrediction(false);
        // Pass the actual risk objects to generateAIExplanation
        await generateAIExplanation({
            age, lifestyleScore, geneticMarker, bloodPressure, cholesterol, sleepHours,
            cReactiveProtein, microbiomeDiversity, telomereLength,
            cardioRisk, metabolicRisk, cognitiveRisk,
            recommendations: Array.from(recommendations)
        });
    };

    // --- AI Explanation Generation ---
    const generateAIExplanation = async (data) => {
        setLoadingExplanation(true);
        try {
            const prompt = `Based on the following simulated biological inputs and risks for a person:
            - Age: ${data.age}
            - Lifestyle Score (1-10): ${data.lifestyleScore}
            - Genetic Marker: ${data.geneticMarker}
            - Systolic Blood Pressure: ${data.bloodPressure} mmHg
            - Total Cholesterol: ${data.cholesterol} mg/dL
            - Average Sleep: ${data.sleepHours} hours
            - C-Reactive Protein (CRP): ${data.cReactiveProtein} mg/L
            - Microbiome Diversity (1-10): ${data.microbiomeDiversity}
            - Telomere Length: ${data.telomereLength}

            Predicted Risks:
            - Cardiovascular Health: ${data.cardioRisk.level}
            - Metabolic Health: ${data.metabolicRisk.level}
            - Cognitive Health: ${data.cognitiveRisk.level}

            Recommendations: ${data.recommendations.join('; ')}.

            Please provide a concise, insightful explanation (max 500 characters) of these predictions. Focus on why certain inputs contribute to specific risks and how the recommendations address them. Use a scientific yet accessible tone, like a bio AI would communicate to a user. Do not include a greeting.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            const result = await response.json();

            if (result.candidates && result.candidates.length > 0 &&
                result.candidates[0].content && result.candidates[0].content.parts &&
                result.candidates[0].content.parts.length > 0) {
                const text = result.candidates[0].content.parts[0].text;
                setAiExplanation(text);
            } else {
                setAiExplanation("AI explanation could not be generated.");
                console.error("AI explanation response structure unexpected:", result);
            }
        } catch (err) {
            setAiExplanation("Error generating AI explanation.");
            console.error("Error fetching AI explanation:", err);
        } finally {
            setLoadingExplanation(false);
        }
    };


    return (
        <div className="space-y-8">
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 bg-blue-50 p-6 rounded-xl shadow-inner">
                {/* Age Input */}
                <div>
                    <label htmlFor="age" className="block text-md font-semibold text-gray-800 mb-2">
                        Age: <span className="text-blue-600 font-bold">{age} years</span>
                    </label>
                    <input type="range" id="age" min="20" max="90" value={age} onChange={(e) => setAge(parseInt(e.target.value))}
                        className="w-full h-2 bg-blue-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Lifestyle Score Input */}
                <div>
                    <label htmlFor="lifestyle" className="block text-md font-semibold text-gray-800 mb-2">
                        Lifestyle Score (1=Poor, 10=Excellent): <span className="text-green-600 font-bold">{lifestyleScore}</span>
                    </label>
                    <input type="range" id="lifestyle" min="1" max="10" value={lifestyleScore} onChange={(e) => setLifestyleScore(parseInt(e.target.value))}
                        className="w-full h-2 bg-green-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Genetic Marker Input */}
                <div>
                    <label htmlFor="genetic-marker" className="block text-md font-semibold text-gray-800 mb-2">
                        Genetic Predisposition:
                    </label>
                    <select id="genetic-marker" value={geneticMarker} onChange={(e) => setGeneticMarker(e.target.value)}
                        className="w-full p-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm text-gray-700" disabled={loadingPrediction}>
                        <option value="none">No specific marker</option>
                        <option value="APOE_e4">APOE e4 (Higher cognitive/cardio risk)</option>
                        <option value="FOXO3_variant">FOXO3 Variant (Potential longevity factor)</option>
                    </select>
                </div>

                {/* Blood Pressure Input */}
                <div>
                    <label htmlFor="blood-pressure" className="block text-md font-semibold text-gray-800 mb-2">
                        Systolic Blood Pressure (mmHg): <span className="text-red-600 font-bold">{bloodPressure}</span>
                    </label>
                    <input type="range" id="blood-pressure" min="90" max="180" value={bloodPressure} onChange={(e) => setBloodPressure(parseInt(e.target.value))}
                        className="w-full h-2 bg-red-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Cholesterol Input */}
                <div>
                    <label htmlFor="cholesterol" className="block text-md font-semibold text-gray-800 mb-2">
                        Total Cholesterol (mg/dL): <span className="text-purple-600 font-bold">{cholesterol}</span>
                    </label>
                    <input type="range" id="cholesterol" min="100" max="300" value={cholesterol} onChange={(e) => setCholesterol(parseInt(e.target.value))}
                        className="w-full h-2 bg-purple-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Sleep Hours Input */}
                <div>
                    <label htmlFor="sleep-hours" className="block text-md font-semibold text-gray-800 mb-2">
                        Average Sleep (hours/night): <span className="text-yellow-600 font-bold">{sleepHours}</span>
                    </label>
                    <input type="range" id="sleep-hours" min="4" max="12" value={sleepHours} onChange={(e) => setSleepHours(parseInt(e.target.value))}
                        className="w-full h-2 bg-yellow-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* C-Reactive Protein (Inflammation Marker) */}
                <div>
                    <label htmlFor="crp" className="block text-md font-semibold text-gray-800 mb-2">
                        C-Reactive Protein (CRP) (mg/L): <span className="text-orange-600 font-bold">{cReactiveProtein}</span>
                    </label>
                    <input type="range" id="crp" min="0.5" max="10" step="0.1" value={cReactiveProtein} onChange={(e) => setCReactiveProtein(parseFloat(e.target.value))}
                        className="w-full h-2 bg-orange-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Microbiome Diversity */}
                <div>
                    <label htmlFor="microbiome" className="block text-md font-semibold text-gray-800 mb-2">
                        Microbiome Diversity (1=Low, 10=High): <span className="text-teal-600 font-bold">{microbiomeDiversity}</span>
                    </label>
                    <input type="range" id="microbiome" min="1" max="10" value={microbiomeDiversity} onChange={(e) => setMicrobiomeDiversity(parseInt(e.target.value))}
                        className="w-full h-2 bg-teal-200 rounded-lg appearance-none cursor-pointer range-lg" disabled={loadingPrediction} />
                </div>

                {/* Telomere Length */}
                <div>
                    <label htmlFor="telomere" className="block text-md font-semibold text-gray-800 mb-2">
                        Telomere Length:
                    </label>
                    <select id="telomere" value={telomereLength} onChange={(e) => setTelomereLength(e.target.value)}
                        className="w-full p-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm text-gray-700" disabled={loadingPrediction}>
                        <option value="average">Average for Age</option>
                        <option value="short">Shorter than Average</option>
                        <option value="long">Longer than Average</option>
                    </select>
                </div>
            </div>

            {/* Prediction Button */}
            <button
                onClick={runPrediction}
                className="w-full py-4 bg-blue-600 text-white text-xl font-semibold rounded-lg shadow-lg hover:bg-blue-700 transition-all duration-300 flex items-center justify-center disabled:opacity-50 disabled:cursor-not-allowed mt-8"
                disabled={loadingPrediction}
            >
                {loadingPrediction ? (
                    <svg className="animate-spin h-6 w-6 mr-3 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                        <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                        <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                    </svg>
                ) : (
                    'Run AI Prediction'
                )}
            </button>

            {/* Error Display */}
            {error && (
                <div className="bg-red-100 border-l-4 border-red-500 text-red-700 p-4 rounded-md mt-6" role="alert">
                    <p className="font-bold">Error:</p>
                    <p>{error}</p>
                </div>
            )}

            {/* Prediction Result Display */}
            {predictionResult && (
                <div className="bg-gray-50 p-6 rounded-lg shadow-inner mt-6 border border-gray-200 space-y-4">
                    <h2 className="text-2xl font-bold text-gray-800 mb-4">AI Prediction Result:</h2>

                    <div className="grid grid-cols-1 sm:grid-cols-3 gap-4 text-center">
                        <div>
                            <p className="text-gray-700 text-sm">Cardiovascular Health Risk</p>
                            <p className={`text-2xl font-extrabold ${predictionResult.cardio.color}`}>{predictionResult.cardio.level}</p>
                        </div>
                        <div>
                            <p className="text-gray-700 text-sm">Metabolic Health Risk</p>
                            <p className={`text-2xl font-extrabold ${predictionResult.metabolic.color}`}>{predictionResult.metabolic.level}</p>
                        </div>
                        <div>
                            <p className="text-gray-700 text-sm">Cognitive Health Risk</p>
                            <p className={`text-2xl font-extrabold ${predictionResult.cognitive.color}`}>{predictionResult.cognitive.level}</p>
                        </div>
                    </div>

                    <h3 className="text-xl font-bold text-gray-800 mb-3">Personalized Recommendations:</h3>
                    <ul className="list-disc list-inside text-gray-700 space-y-2">
                        {predictionResult.recommendations.map((rec, index) => (
                            <li key={index}>{rec}</li>
                        ))}
                    </ul>

                    {/* AI Explanation Section */}
                    <h3 className="text-xl font-bold text-gray-800 mt-6 mb-3">AI Explanation:</h3>
                    {loadingExplanation ? (
                        <div className="flex items-center justify-center p-4 bg-white rounded-md shadow-sm">
                            <svg className="animate-spin h-5 w-5 mr-3 text-blue-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                                <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                                <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                            </svg>
                            <p className="text-gray-600">Generating AI explanation...</p>
                        </div>
                    ) : (
                        <div className="bg-white p-4 rounded-md shadow-sm text-gray-800 border border-gray-100">
                            {aiExplanation || "No explanation generated yet."}
                        </div>
                    )}

                    <p className="text-gray-500 text-sm mt-4">
                        *This AI is a simulation. Always consult with a qualified healthcare professional for medical advice.
                    </p>
                </div>
            )}
        </div>
    );
};

// Component for Goals and Protocols
const GoalsAndProtocols = () => {
    const [healthGoal, setHealthGoal] = useState('');
    const [protocolDescription, setProtocolDescription] = useState('');
    const [protocolFeedback, setProtocolFeedback] = useState('');
    const [loadingFeedback, setLoadingFeedback] = useState(false);
    const [error, setError] = useState('');
    const [savedProtocols, setSavedProtocols] = useState([]);

    const generateProtocolFeedback = async () => {
        setLoadingFeedback(true);
        setProtocolFeedback('');
        setError('');

        if (!protocolDescription.trim()) {
            setError("Please describe your protocol.");
            setLoadingFeedback(false);
            return;
        }

        try {
            const prompt = `Evaluate the following longevity protocol proposed by a user: "${protocolDescription}". The user's stated health goal is: "${healthGoal || 'general longevity and healthspan improvement'}". Provide feedback on its potential effectiveness for improving healthspan and reaching the goal. Suggest potential benefits, drawbacks, and areas for refinement based on general longevity science principles. Be encouraging but realistic. Max 500 characters. Do not include a greeting.`;

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });
            const payload = { contents: chatHistory };
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            const result = await response.json();

            if (result.candidates && result.candidates.length > 0 &&
                result.candidates[0].content && result.candidates[0].content.parts &&
                result.candidates[0].content.parts.length > 0) {
                const text = result.candidates[0].content.parts[0].text;
                setProtocolFeedback(text);
                setSavedProtocols(prev => [
                    { id: Date.now(), goal: healthGoal, protocol: protocolDescription, feedback: text },
                    ...prev
                ]);
                setHealthGoal('');
                setProtocolDescription('');
            } else {
                setProtocolFeedback("AI feedback could not be generated.");
                console.error("AI feedback response structure unexpected:", result);
            }
        } catch (err) {
            setProtocolFeedback("Error generating AI feedback.");
            console.error("Error fetching AI feedback:", err);
        } finally {
            setLoadingFeedback(false);
        }
    };

    return (
        <div className="space-y-8">
            <div className="bg-green-50 p-6 rounded-xl shadow-inner space-y-6">
                <h2 className="text-2xl font-bold text-gray-800 mb-4">Set Your Goals & Submit Protocols</h2>
                <p className="text-gray-600">
                    Tell the AI your health goals and describe longevity protocols for personalized feedback.
                </p>

                {/* Health Goal Input */}
                <div>
                    <label htmlFor="health-goal" className="block text-md font-semibold text-gray-800 mb-2">
                        Your Longevity Health Goal (Optional):
                    </label>
                    <input
                        type="text"
                        id="health-goal"
                        value={healthGoal}
                        onChange={(e) => setHealthGoal(e.target.value)}
                        placeholder="e.g., Reduce inflammation, Improve cognitive function, Extend healthspan"
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-green-500 focus:border-green-500 shadow-sm text-gray-700"
                        disabled={loadingFeedback}
                    />
                </div>

                {/* Protocol Description */}
                <div>
                    <label htmlFor="protocol-desc" className="block text-md font-semibold text-gray-800 mb-2">
                        Describe Your Protocol:
                    </label>
                    <textarea
                        id="protocol-desc"
                        value={protocolDescription}
                        onChange={(e) => setProtocolDescription(e.target.value)}
                        placeholder="e.g., 'Daily cold plunges, 16/8 intermittent fasting, 500mg NMN daily, strength training 3x/week.'"
                        rows="5"
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-green-500 focus:border-green-500 shadow-sm text-gray-700 resize-y"
                        disabled={loadingFeedback}
                    ></textarea>
                </div>

                <button
                    onClick={generateProtocolFeedback}
                    className="w-full py-4 bg-green-600 text-white text-xl font-semibold rounded-lg shadow-lg hover:bg-green-700 transition-all duration-300 flex items-center justify-center disabled:opacity-50 disabled:cursor-not-allowed"
                    disabled={loadingFeedback}
                >
                    {loadingFeedback ? (
                        <svg className="animate-spin h-6 w-6 mr-3 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                            <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                            <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                        </svg>
                    ) : (
                        'Get AI Protocol Feedback'
                    )}
                </button>

                {error && (
                    <div className="bg-red-100 border-l-4 border-red-500 text-red-700 p-4 rounded-md mt-6" role="alert">
                        <p className="font-bold">Error:</p>
                        <p>{error}</p>
                    </div>
                )}

                {protocolFeedback && (
                    <div className="bg-white p-4 rounded-md shadow-sm text-gray-800 border border-gray-100 mt-6">
                        <h3 className="text-xl font-bold text-gray-800 mb-2">AI Feedback:</h3>
                        <p>{protocolFeedback}</p>
                    </div>
                )}
            </div>

            {/* Saved Protocols History */}
            {savedProtocols.length > 0 && (
                <div className="bg-gray-50 p-6 rounded-xl shadow-inner mt-8 border border-gray-200">
                    <h3 className="text-2xl font-bold text-gray-800 mb-4">Your Protocol History:</h3>
                    <div className="space-y-4">
                        {savedProtocols.map(p => (
                            <div key={p.id} className="bg-white p-4 rounded-md shadow-sm border border-gray-100">
                                <p className="text-sm text-gray-500">Goal: <span className="font-semibold text-gray-700">{p.goal || 'General Healthspan'}</span></p>
                                <p className="text-sm text-gray-500">Protocol:</p>
                                <p className="text-gray-800 font-medium whitespace-pre-wrap mb-2">{p.protocol}</p>
                                <h4 className="text-md font-bold text-blue-700">AI Feedback:</h4>
                                <p className="text-gray-700 text-sm whitespace-pre-wrap">{p.feedback}</p>
                            </div>
                        ))}
                    </div>
                </div>
            )}
        </div>
    );
};

export default App;

import React, { useState, useEffect } from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer, BarChart, Bar } from 'recharts';
import { Heart, Upload, AlertTriangle, CheckCircle, XCircle, MessageCircle, TrendingUp, Activity } from 'lucide-react';

const HeartHealthApp = () => {
  const [data, setData] = useState([]);
  const [analysis, setAnalysis] = useState(null);
  const [chatMessages, setChatMessages] = useState([]);
  const [userInput, setUserInput] = useState('');
  const [activeTab, setActiveTab] = useState('upload');

  // Sample data generator for demo
  const generateSampleData = () => {
    const sampleData = [];
    for (let i = 0; i < 30; i++) {
      sampleData.push({
        date: new Date(Date.now() - (29 - i) * 24 * 60 * 60 * 1000).toISOString().split('T')[0],
        heartRate: Math.floor(Math.random() * 40 + 60), // 60-100 bpm
        steps: Math.floor(Math.random() * 5000 + 5000), // 5k-10k steps
        sleep: Math.random() * 3 + 6, // 6-9 hours
        restingHR: Math.floor(Math.random() * 20 + 55), // 55-75 bpm
        hrv: Math.floor(Math.random() * 30 + 20), // 20-50 ms
        bloodOxygen: Math.random() * 2 + 97, // 97-99%
      });
    }
    setData(sampleData);
    analyzeHealthData(sampleData);
  };

  const analyzeHealthData = (healthData) => {
    if (!healthData || healthData.length === 0) return;

    const avgHeartRate = healthData.reduce((sum, d) => sum + d.heartRate, 0) / healthData.length;
    const avgRestingHR = healthData.reduce((sum, d) => sum + d.restingHR, 0) / healthData.length;
    const avgHRV = healthData.reduce((sum, d) => sum + d.hrv, 0) / healthData.length;
    const avgSleep = healthData.reduce((sum, d) => sum + d.sleep, 0) / healthData.length;
    const avgSteps = healthData.reduce((sum, d) => sum + d.steps, 0) / healthData.length;

    // Simple risk scoring algorithm
    let riskScore = 0;
    let factors = [];

    // Heart rate analysis
    if (avgRestingHR > 80) {
      riskScore += 2;
      factors.push('Elevated resting heart rate');
    }
    if (avgHeartRate > 90) {
      riskScore += 1;
      factors.push('High average heart rate');
    }

    // HRV analysis (lower is worse for heart health)
    if (avgHRV < 25) {
      riskScore += 2;
      factors.push('Low heart rate variability');
    }

    // Sleep analysis
    if (avgSleep < 6.5) {
      riskScore += 1;
      factors.push('Insufficient sleep');
    }

    // Activity analysis
    if (avgSteps < 7000) {
      riskScore += 1;
      factors.push('Low daily activity');
    }

    // Risk categorization
    let riskLevel, recommendation, color;
    if (riskScore <= 1) {
      riskLevel = 'Normal';
      recommendation = 'Your heart health indicators look good! Keep up the healthy lifestyle.';
      color = 'text-green-600';
    } else if (riskScore <= 3) {
      riskLevel = 'Monitor';
      recommendation = 'Some indicators suggest you should monitor your heart health more closely. Consider a checkup with your doctor.';
      color = 'text-yellow-600';
    } else {
      riskLevel = 'Urgent';
      recommendation = 'Multiple risk factors detected. Please consult with a healthcare professional soon.';
      color = 'text-red-600';
    }

    setAnalysis({
      riskScore,
      riskLevel,
      recommendation,
      color,
      factors,
      metrics: {
        avgHeartRate: Math.round(avgHeartRate),
        avgRestingHR: Math.round(avgRestingHR),
        avgHRV: Math.round(avgHRV),
        avgSleep: avgSleep.toFixed(1),
        avgSteps: Math.round(avgSteps)
      }
    });
  };

  const handleFileUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        try {
          // Simple CSV parsing - in production you'd use a proper CSV parser
          const text = e.target.result;
          const lines = text.split('\n');
          const headers = lines[0].split(',');
          
          const parsedData = lines.slice(1).map(line => {
            const values = line.split(',');
            return headers.reduce((obj, header, index) => {
              obj[header.trim()] = isNaN(values[index]) ? values[index] : Number(values[index]);
              return obj;
            }, {});
          }).filter(row => Object.keys(row).length > 1);

          setData(parsedData);
          analyzeHealthData(parsedData);
        } catch (error) {
          alert('Error parsing file. Please ensure it\'s a valid CSV with health data.');
        }
      };
      reader.readAsText(file);
    }
  };

  const sendMessage = () => {
    if (!userInput.trim()) return;
    
    const newMessage = { role: 'user', content: userInput };
    const responses = [
      "Based on your data, your average resting heart rate is in a healthy range. This is a good indicator of cardiovascular fitness!",
      "I notice some variability in your sleep patterns. Consistent sleep is crucial for heart health - aim for 7-8 hours nightly.",
      "Your heart rate variability suggests good autonomic nervous system function. This is associated with better stress resilience.",
      "The step count data shows you're fairly active. Regular movement is one of the best things for heart health!",
      "Your blood oxygen levels look normal. This indicates good oxygen transport efficiency in your cardiovascular system."
    ];
    
    const botResponse = { 
      role: 'assistant', 
      content: responses[Math.floor(Math.random() * responses.length)]
    };

    setChatMessages(prev => [...prev, newMessage, botResponse]);
    setUserInput('');
  };

  const getRiskIcon = () => {
    if (!analysis) return <Heart className="w-6 h-6" />;
    switch (analysis.riskLevel) {
      case 'Normal': return <CheckCircle className="w-6 h-6 text-green-600" />;
      case 'Monitor': return <AlertTriangle className="w-6 h-6 text-yellow-600" />;
      case 'Urgent': return <XCircle className="w-6 h-6 text-red-600" />;
      default: return <Heart className="w-6 h-6" />;
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
      <div className="container mx-auto p-6">
        {/* Header */}
        <div className="text-center mb-8">
          <div className="flex justify-center items-center gap-3 mb-4">
            <Heart className="w-10 h-10 text-red-500" />
            <h1 className="text-4xl font-bold text-gray-800">Heart Health Monitor</h1>
          </div>
          <p className="text-gray-600 max-w-2xl mx-auto">
            Upload your wearable data to get insights into your cardiovascular health. 
            AI-powered analysis helps you understand your heart health patterns.
          </p>
          <div className="mt-4 p-3 bg-yellow-50 border border-yellow-200 rounded-lg max-w-2xl mx-auto">
            <p className="text-yellow-800 text-sm">
              ⚠️ This is for educational purposes only and not medical advice. Always consult healthcare professionals.
            </p>
          </div>
        </div>

        {/* Navigation */}
        <div className="flex justify-center mb-6">
          <div className="bg-white rounded-lg shadow-md p-1">
            {['upload', 'dashboard', 'analysis', 'chat'].map((tab) => (
              <button
                key={tab}
                onClick={() => setActiveTab(tab)}
                className={`px-6 py-2 rounded-md capitalize transition-colors ${
                  activeTab === tab 
                    ? 'bg-blue-500 text-white' 
                    : 'text-gray-600 hover:bg-gray-100'
                }`}
              >
                {tab}
              </button>
            ))}
          </div>
        </div>

        {/* Upload Tab */}
        {activeTab === 'upload' && (
          <div className="max-w-2xl mx-auto">
            <div className="bg-white rounded-lg shadow-lg p-8">
              <div className="text-center">
                <Upload className="w-16 h-16 text-blue-500 mx-auto mb-4" />
                <h2 className="text-2xl font-semibold mb-4">Upload Your Wearable Data</h2>
                <p className="text-gray-600 mb-6">
                  Upload CSV data from your smartwatch, fitness tracker, or health app
                </p>
                
                <input
                  type="file"
                  accept=".csv"
                  onChange={handleFileUpload}
                  className="mb-4 block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100"
                />
                
                <div className="text-center">
                  <p className="text-gray-500 mb-4">Or try with sample data:</p>
                  <button
                    onClick={generateSampleData}
                    className="bg-green-500 hover:bg-green-600 text-white px-6 py-3 rounded-lg font-semibold transition-colors"
                  >
                    Generate Sample Data
                  </button>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* Dashboard Tab */}
        {activeTab === 'dashboard' && data.length > 0 && (
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
            {/* Heart Rate Chart */}
            <div className="bg-white rounded-lg shadow-lg p-6">
              <h3 className="text-xl font-semibold mb-4 flex items-center gap-2">
                <Activity className="w-5 h-5 text-red-500" />
                Heart Rate Trends
              </h3>
              <ResponsiveContainer width="100%" height={300}>
                <LineChart data={data}>
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis dataKey="date" />
                  <YAxis />
                  <Tooltip />
                  <Legend />
                  <Line type="monotone" dataKey="heartRate" stroke="#ef4444" strokeWidth={2} />
                  <Line type="monotone" dataKey="restingHR" stroke="#f97316" strokeWidth={2} />
                </LineChart>
              </ResponsiveContainer>
            </div>

            {/* Activity Chart */}
            <div className="bg-white rounded-lg shadow-lg p-6">
              <h3 className="text-xl font-semibold mb-4 flex items-center gap-2">
                <TrendingUp className="w-5 h-5 text-blue-500" />
                Daily Activity
              </h3>
              <ResponsiveContainer width="100%" height={300}>
                <BarChart data={data.slice(-14)}>
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis dataKey="date" />
                  <YAxis />
                  <Tooltip />
                  <Legend />
                  <Bar dataKey="steps" fill="#3b82f6" />
                </BarChart>
              </ResponsiveContainer>
            </div>

            {/* Sleep & HRV Chart */}
            <div className="bg-white rounded-lg shadow-lg p-6">
              <h3 className="text-xl font-semibold mb-4">Sleep & Recovery</h3>
              <ResponsiveContainer width="100%" height={300}>
                <LineChart data={data}>
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis dataKey="date" />
                  <YAxis />
                  <Tooltip />
                  <Legend />
                  <Line type="monotone" dataKey="sleep" stroke="#10b981" strokeWidth={2} />
                  <Line type="monotone" dataKey="hrv" stroke="#8b5cf6" strokeWidth={2} />
                </LineChart>
              </ResponsiveContainer>
            </div>

            {/* Blood Oxygen */}
            <div className="bg-white rounded-lg shadow-lg p-6">
              <h3 className="text-xl font-semibold mb-4">Blood Oxygen Levels</h3>
              <ResponsiveContainer width="100%" height={300}>
                <LineChart data={data}>
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis dataKey="date" />
                  <YAxis domain={[95, 100]} />
                  <Tooltip />
                  <Legend />
                  <Line type="monotone" dataKey="bloodOxygen" stroke="#06b6d4" strokeWidth={2} />
                </LineChart>
              </ResponsiveContainer>
            </div>
          </div>
        )}

        {/* Analysis Tab */}
        {activeTab === 'analysis' && analysis && (
          <div className="max-w-4xl mx-auto">
            <div className="bg-white rounded-lg shadow-lg p-8">
              <div className="text-center mb-8">
                <div className="flex justify-center items-center gap-3 mb-4">
                  {getRiskIcon()}
                  <h2 className="text-3xl font-semibold">Health Analysis</h2>
                </div>
                <div className={`text-2xl font-bold ${analysis.color} mb-2`}>
                  Risk Level: {analysis.riskLevel}
                </div>
                <p className="text-gray-600 max-w-2xl mx-auto">{analysis.recommendation}</p>
              </div>

              {/* Metrics Grid */}
              <div className="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8">
                <div className="bg-blue-50 rounded-lg p-4 text-center">
                  <div className="text-2xl font-bold text-blue-600">{analysis.metrics.avgHeartRate}</div>
                  <div className="text-sm text-gray-600">Avg Heart Rate</div>
                </div>
                <div className="bg-orange-50 rounded-lg p-4 text-center">
                  <div className="text-2xl font-bold text-orange-600">{analysis.metrics.avgRestingHR}</div>
                  <div className="text-sm text-gray-600">Resting HR</div>
                </div>
                <div className="bg-purple-50 rounded-lg p-4 text-center">
                  <div className="text-2xl font-bold text-purple-600">{analysis.metrics.avgHRV}</div>
                  <div className="text-sm text-gray-600">HRV (ms)</div>
                </div>
                <div className="bg-green-50 rounded-lg p-4 text-center">
                  <div className="text-2xl font-bold text-green-600">{analysis.metrics.avgSleep}h</div>
                  <div className="text-sm text-gray-600">Avg Sleep</div>
                </div>
                <div className="bg-indigo-50 rounded-lg p-4 text-center">
                  <div className="text-2xl font-bold text-indigo-600">{analysis.metrics.avgSteps.toLocaleString()}</div>
                  <div className="text-sm text-gray-600">Daily Steps</div>
                </div>
              </div>

              {/* Risk Factors */}
              {analysis.factors.length > 0 && (
                <div className="bg-yellow-50 border border-yellow-200 rounded-lg p-6">
                  <h3 className="text-lg font-semibold mb-3 text-yellow-800">Areas to Monitor:</h3>
                  <ul className="space-y-2">
                    {analysis.factors.map((factor, index) => (
                      <li key={index} className="flex items-center gap-2 text-yellow-700">
                        <AlertTriangle className="w-4 h-4" />
                        {factor}
                      </li>
                    ))}
                  </ul>
                </div>
              )}
            </div>
          </div>
        )}

        {/* Chat Tab */}
        {activeTab === 'chat' && (
          <div className="max-w-2xl mx-auto">
            <div className="bg-white rounded-lg shadow-lg p-6">
              <div className="flex items-center gap-3 mb-6">
                <MessageCircle className="w-6 h-6 text-blue-500" />
                <h2 className="text-2xl font-semibold">AI Health Assistant</h2>
              </div>
              
              <div className="h-96 overflow-y-auto bg-gray-50 rounded-lg p-4 mb-4">
                {chatMessages.length === 0 ? (
                  <div className="text-center text-gray-500 mt-8">
                    <p>Ask me about your heart health data!</p>
                    <p className="text-sm mt-2">Try: "What does my heart rate variability mean?"</p>
                  </div>
                ) : (
                  chatMessages.map((msg, index) => (
                    <div key={index} className={`mb-4 ${msg.role === 'user' ? 'text-right' : 'text-left'}`}>
                      <div className={`inline-block p-3 rounded-lg max-w-xs ${
                        msg.role === 'user' 
                          ? 'bg-blue-500 text-white' 
                          : 'bg-white border border-gray-200'
                      }`}>
                        {msg.content}
                      </div>
                    </div>
                  ))
                )}
              </div>
              
              <div className="flex gap-2">
                <input
                  type="text"
                  value={userInput}
                  onChange={(e) => setUserInput(e.target.value)}
                  onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
                  placeholder="Ask about your heart health data..."
                  className="flex-1 p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
                <button
                  onClick={sendMessage}
                  className="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold transition-colors"
                >
                  Send
                </button>
              </div>
            </div>
          </div>
        )}

        {/* No Data State */}
        {activeTab !== 'upload' && data.length === 0 && (
          <div className="text-center py-12">
            <Heart className="w-16 h-16 text-gray-400 mx-auto mb-4" />
            <h3 className="text-xl font-semibold text-gray-600 mb-2">No Data Yet</h3>
            <p className="text-gray-500 mb-4">Upload your wearable data to see insights and analysis</p>
            <button
              onClick={() => setActiveTab('upload')}
              className="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold transition-colors"
            >
              Upload Data
            </button>
          </div>
        )}
      </div>
    </div>
  );
};

export default HeartHealthApp;

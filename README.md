import React, { useState, useEffect } from 'react';
import { Calculator } from 'lucide-react';

export default function WeldingHeatCalculator() {
  const [weldLength, setWeldLength] = useState('');
  const [arcTime, setArcTime] = useState('');
  const [current, setCurrent] = useState('');
  const [voltage, setVoltage] = useState('');
  const [weldSpeed, setWeldSpeed] = useState(null);
  const [heatInput, setHeatInput] = useState(null);

  const handleClear = () => {
    setWeldLength('');
    setArcTime('');
    setCurrent('');
    setVoltage('');
    setWeldSpeed(null);
    setHeatInput(null);
  };

  useEffect(() => {
    if (weldLength && arcTime) {
      const length = parseFloat(weldLength);
      const time = parseFloat(arcTime);
      if (length > 0 && time > 0) {
        const speed = (60 / time) * length;
        setWeldSpeed(speed);
      } else {
        setWeldSpeed(null);
      }
    } else {
      setWeldSpeed(null);
    }
  }, [weldLength, arcTime]);

  useEffect(() => {
    if (weldSpeed && current && voltage) {
      const A = parseFloat(current);
      const V = parseFloat(voltage);
      if (A > 0 && V > 0 && weldSpeed > 0) {
        const H = (A * V * 60) / weldSpeed;
        setHeatInput(H);
      } else {
        setHeatInput(null);
      }
    } else {
      setHeatInput(null);
    }
  }, [weldSpeed, current, voltage]);

  return (
    <div className="min-h-screen bg-gradient-to-br from-pink-200 via-purple-200 to-blue-200 p-4">
      <div className="max-w-md mx-auto">
        <div className="bg-white rounded-3xl shadow-2xl p-6 border-4 border-purple-300">
          <div className="flex flex-col items-center justify-center mb-4">
            <Calculator className="w-16 h-16 text-purple-500 mb-2" />
            <h1 className="text-4xl font-bold bg-gradient-to-r from-purple-600 to-pink-600 bg-clip-text text-transparent text-center">THE truth</h1>
          </div>
          <p className="text-center text-purple-600 mb-6 text-lg font-semibold">✨ 入熱の真実 ✨</p>

          <button
            onClick={handleClear}
            className="w-full mb-5 px-4 py-3 bg-gradient-to-r from-red-400 to-pink-400 text-white font-bold rounded-xl shadow-lg hover:from-red-500 hover:to-pink-500 active:scale-95 transition-all duration-200 border-2 border-red-300"
          >
            🗑️ クリア
          </button>

          <div className="space-y-5">
            <div className="bg-gradient-to-r from-blue-100 to-cyan-100 p-5 rounded-2xl border-2 border-blue-300 shadow-lg">
              <h2 className="text-xl font-bold text-blue-700 mb-3 text-center">
                🚀 溶接速度の計算
              </h2>
              
              <div className="space-y-3">
                <div>
                  <label className="block text-base font-medium text-gray-700 mb-1">
                    1. 溶接長 (cm)
                  </label>
                  <input
                    type="number"
                    value={weldLength}
                    onChange={(e) => setWeldLength(e.target.value)}
                    className="w-full px-4 py-3 border-2 border-blue-300 rounded-xl focus:ring-2 focus:ring-blue-400 focus:border-blue-400 text-base"
                    placeholder="溶接長を入力"
                  />
                </div>

                <div>
                  <label className="block text-base font-medium text-gray-700 mb-1">
                    2. アークタイム (sec)
                  </label>
                  <input
                    type="number"
                    value={arcTime}
                    onChange={(e) => setArcTime(e.target.value)}
                    className="w-full px-4 py-3 border-2 border-blue-300 rounded-xl focus:ring-2 focus:ring-blue-400 focus:border-blue-400 text-base"
                    placeholder="アークタイムを入力"
                  />
                </div>
              </div>

              {weldSpeed !== null && (
                <div className="mt-3 p-3 bg-gradient-to-r from-green-200 to-emerald-200 rounded-xl border-2 border-green-400 shadow-md">
                  <p className="text-sm text-green-700 font-semibold text-center">💨 溶接速度 (v)</p>
                  <p className="text-2xl font-bold text-green-700 text-center">
                    {weldSpeed.toFixed(2)} cm/min
                  </p>
                </div>
              )}
            </div>

            <div className="bg-gradient-to-r from-orange-100 to-yellow-100 p-5 rounded-2xl border-2 border-orange-300 shadow-lg">
              <h2 className="text-xl font-bold text-orange-700 mb-3 text-center">
                ⚡ 入熱量の計算
              </h2>
              
              <div className="space-y-3">
                <div>
                  <label className="block text-base font-medium text-gray-700 mb-1">
                    3. 電流値 (A)
                  </label>
                  <input
                    type="number"
                    value={current}
                    onChange={(e) => setCurrent(e.target.value)}
                    className="w-full px-4 py-3 border-2 border-orange-300 rounded-xl focus:ring-2 focus:ring-orange-400 focus:border-orange-400 text-base"
                    placeholder="電流値を入力"
                  />
                </div>

                <div>
                  <label className="block text-base font-medium text-gray-700 mb-1">
                    4. 電圧値 (V)
                  </label>
                  <input
                    type="number"
                    value={voltage}
                    onChange={(e) => setVoltage(e.target.value)}
                    className="w-full px-4 py-3 border-2 border-orange-300 rounded-xl focus:ring-2 focus:ring-orange-400 focus:border-orange-400 text-base"
                    placeholder="電圧値を入力"
                  />
                </div>
              </div>

              {heatInput !== null && (
                <div className="mt-3 space-y-2">
                  <div className="p-3 bg-gradient-to-r from-red-200 to-pink-200 rounded-xl border-2 border-red-400 shadow-md">
                    <p className="text-sm text-red-700 font-semibold text-center">🔥 入熱量 (H)</p>
                    <p className="text-2xl font-bold text-red-700 text-center">
                      {heatInput.toFixed(2)} J/cm
                    </p>
                  </div>
                  <div className="p-3 bg-gradient-to-r from-purple-200 to-indigo-200 rounded-xl border-2 border-purple-400 shadow-md">
                    <p className="text-xl font-bold text-purple-700 text-center">
                      {(heatInput / 1000).toFixed(1)} kJ/cm
                    </p>
                  </div>
                </div>
              )}
            </div>

            <div className="bg-gradient-to-r from-purple-100 to-pink-100 p-4 rounded-2xl text-xs text-purple-700 border-2 border-purple-300">
              <p className="font-bold mb-2 text-sm text-center">📐 計算式</p>
              <p className="mb-1">溶接速度 (v) = (60 / アークタイム) × 溶接長</p>
              <p>入熱量 (H) = 電流値 × 電圧値 × 60 / 溶接速度</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

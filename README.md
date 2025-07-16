import React from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer, Area, AreaChart } from 'recharts';

const InventoryGraph = () => {
  // Generar datos para la función D(t) = 50 + 10t - 0.5t²
  const generateData = () => {
    const data = [];
    for (let t = 0; t <= 10; t += 0.2) {
      const demand = 50 + 10 * t - 0.5 * t * t;
      data.push({
        t: parseFloat(t.toFixed(1)),
        demand: parseFloat(demand.toFixed(2))
      });
    }
    return data;
  };

  const data = generateData();

  // Calcular algunos puntos clave
  const keyPoints = [
    { t: 0, demand: 50, label: 'Día 0: 50 MCUs/día' },
    { t: 5, demand: 87.5, label: 'Día 5: 87.5 MCUs/día' },
    { t: 10, demand: 100, label: 'Día 10: 100 MCUs/día' }
  ];

  return (
    <div className="w-full p-6 bg-white">
      <h2 className="text-2xl font-bold text-center mb-6 text-gray-800">
        Optimización de Inventario de Microcontroladores
      </h2>
      
      <div className="mb-6 bg-blue-50 p-4 rounded-lg">
        <h3 className="text-lg font-semibold mb-2">Función de Demanda</h3>
        <p className="text-lg font-mono bg-white p-2 rounded border">
          D(t) = 50 + 10t - 0.5t²
        </p>
        <p className="text-sm text-gray-600 mt-2">
          Donde t es el tiempo en días (0 ≤ t ≤ 10)
        </p>
        <div className="mt-3 p-3 bg-blue-100 rounded border-l-4 border-blue-500">
          <p className="font-semibold text-blue-800">
            🎯 El área bajo esta curva = 834 microcontroladores
          </p>
        </div>
      </div>

      <div className="mb-6">
        <h3 className="text-lg font-semibold mb-4">Gráfico de Demanda Diaria</h3>
        <ResponsiveContainer width="100%" height={400}>
          <AreaChart data={data} margin={{ top: 20, right: 30, left: 20, bottom: 5 }}>
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis 
              dataKey="t" 
              label={{ value: 'Tiempo (días)', position: 'insideBottom', offset: -5 }}
            />
            <YAxis 
              label={{ value: 'Demanda (MCUs/día)', angle: -90, position: 'insideLeft' }}
            />
            <Tooltip 
              formatter={(value) => [`${value} MCUs/día`, 'Demanda']}
              labelFormatter={(label) => `Día ${label}`}
            />
            <Area 
              type="monotone" 
              dataKey="demand" 
              stroke="#2563eb" 
              fill="#3b82f6" 
              fillOpacity={0.3}
              strokeWidth={3}
            />
          </AreaChart>
        </ResponsiveContainer>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div className="bg-green-50 p-4 rounded-lg">
          <h3 className="text-lg font-semibold mb-3 text-green-800">Puntos Clave</h3>
          <ul className="space-y-2">
            {keyPoints.map((point, index) => (
              <li key={index} className="flex items-center">
                <div className="w-3 h-3 bg-green-500 rounded-full mr-3"></div>
                <span className="text-sm font-mono">{point.label}</span>
              </li>
            ))}
          </ul>
        </div>

        <div className="bg-orange-50 p-4 rounded-lg">
          <h3 className="text-lg font-semibold mb-3 text-orange-800">Cálculo de la Integral</h3>
          <div className="space-y-2 text-sm">
            <p className="font-mono">∫₀¹⁰ (50 + 10t - 0.5t²) dt</p>
            <p className="font-mono">= [50t + 5t² - t³/6]₀¹⁰</p>
            <p className="font-mono">= 833.33 MCUs</p>
            <div className="bg-orange-100 p-3 rounded mt-3 border-l-4 border-orange-500">
              <p className="font-bold text-orange-800 text-lg">
                📊 RESULTADO FINAL
              </p>
              <p className="font-bold text-orange-700 text-xl">
                Total requerido: 834 MCUs
              </p>
              <p className="text-orange-600 text-sm mt-1">
                (Área bajo la curva = Demanda total)
              </p>
            </div>
          </div>
        </div>
      </div>

      <div className="mt-6 bg-gray-50 p-4 rounded-lg">
        <h3 className="text-lg font-semibold mb-3">Interpretación del Gráfico</h3>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
          <div>
            <h4 className="font-medium mb-2">Características de la Demanda:</h4>
            <ul className="space-y-1 text-gray-700">
              <li>• Crecimiento cuadrático</li>
              <li>• Demanda mínima: 50 MCUs/día (día 0)</li>
              <li>• Demanda máxima: 100 MCUs/día (día 10)</li>
              <li>• Aceleración hacia el final del ciclo</li>
            </ul>
          </div>
          <div>
            <h4 className="font-medium mb-2">Implicaciones para el Inventario:</h4>
            <ul className="space-y-1 text-gray-700">
              <li>• El área bajo la curva = demanda total</li>
              <li>• Mayor stock necesario en días finales</li>
              <li>• Planificación escalonada recomendada</li>
              <li>• <span className="font-bold text-green-700">Total: 834 microcontroladores</span></li>
            </ul>
          </div>
        </div>
        
        <div className="mt-4 p-4 bg-green-100 rounded-lg border-2 border-green-300">
          <h4 className="font-bold text-green-800 text-lg mb-2">
            ✅ CONCLUSIÓN PARA ELECTROFUTURO S.A.
          </h4>
          <p className="text-green-700 font-semibold">
            La empresa debe pedir <span className="text-xl font-bold">834 microcontroladores</span> 
            para cubrir completamente la demanda durante los 10 días del ciclo de producción.
          </p>
          <p className="text-green-600 text-sm mt-2">
            Este resultado se obtuvo calculando la integral definida ∫₀¹⁰ D(t) dt = 833.33 ≈ 834 MCUs
          </p>
        </div>
      </div>
    </div>
  );
};

export default InventoryGraph;

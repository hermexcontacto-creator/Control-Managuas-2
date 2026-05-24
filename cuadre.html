import React, { useState, useEffect } from 'react';
import { Calculator, Droplet, CheckCircle, AlertCircle, Info, ArrowRight, ShieldCheck, Users, ChevronDown, ChevronUp, Receipt, Wallet, ArrowDownCircle, Clock, Check, CreditCard } from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from 'firebase/auth';
import { getFirestore, doc, setDoc, getDoc } from 'firebase/firestore';

// 1. Inicialización estricta de Firebase
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';

export default function App() {
  const [user, setUser] = useState(null);
  
  const [amount1, setAmount1] = useState('');
  const [amount2, setAmount2] = useState('');
  const [tasaBcv, setTasaBcv] = useState('');
  const [bcvStatus, setBcvStatus] = useState('loading'); // 'loading', 'current', 'old', 'empty'
  
  const [resultado, setResultado] = useState(null);
  const [isCalculating, setIsCalculating] = useState(false);
  const [expandedTransactions, setExpandedTransactions] = useState(false);
  
  // Estados para el contador de billetes
  const [isCounterOpen, setIsCounterOpen] = useState(false);
  const [bills, setBills] = useState({
    500: '', 200: '', 100: '', 50: '', 20: '', 10: ''
  });

  // Estados para sumar Puntos de Venta
  const [isPosOpen, setIsPosOpen] = useState(false);
  const [posBanesco, setPosBanesco] = useState('');
  const [posVenezuela, setPosVenezuela] = useState('');

  // 2. Efecto de Autenticación (Regla estricta)
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (error) {
        console.error("Auth error:", error);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, setUser);
    return () => unsubscribe();
  }, []);

  // 3. Cargar la tasa BCV de Firestore al iniciar sesión
  useEffect(() => {
    if (!user) return;

    const loadBcvData = async () => {
      try {
        const docRef = doc(db, 'artifacts', appId, 'users', user.uid, 'settings', 'bcvData');
        const docSnap = await getDoc(docRef);
        
        if (docSnap.exists()) {
          const data = docSnap.data();
          setTasaBcv(data.rate);
          checkBcvDate(data.timestamp);
        } else {
          setBcvStatus('empty');
        }
      } catch (error) {
        console.error("Error loading BCV:", error);
      }
    };
    
    loadBcvData();
  }, [user]);

  // Función para verificar si la tasa es de hoy (Hora Venezuela)
  const checkBcvDate = (timestamp) => {
    const options = { timeZone: 'America/Caracas', year: 'numeric', month: 'numeric', day: 'numeric' };
    const savedDateStr = new Date(timestamp).toLocaleDateString('en-US', options);
    const currentDateStr = new Date().toLocaleDateString('en-US', options);

    if (savedDateStr === currentDateStr) {
      setBcvStatus('current');
    } else {
      setBcvStatus('old');
    }
  };

  // 4. Guardar tasa en Firestore
  const saveBcvToFirebase = async (rate) => {
    if (!user || !rate) return;
    try {
      const docRef = doc(db, 'artifacts', appId, 'users', user.uid, 'settings', 'bcvData');
      const now = Date.now();
      await setDoc(docRef, {
        rate: rate,
        timestamp: now
      });
      checkBcvDate(now);
    } catch (error) {
      console.error("Error saving BCV:", error);
    }
  };

  // 5. Manejar el input de BCV (Formateo automático de comas a puntos)
  const handleBcvChange = (e) => {
    let val = e.target.value;
    
    // Reemplazar coma por punto
    val = val.replace(/,/g, '.');
    
    // Remover cualquier cosa que no sea número o punto
    val = val.replace(/[^0-9.]/g, '');
    
    // Evitar múltiples puntos (ej: 48.33.22 -> 48.3322)
    const parts = val.split('.');
    if (parts.length > 2) {
      val = parts[0] + '.' + parts.slice(1).join('');
    }
    
    setTasaBcv(val);
    if (!val) setBcvStatus('empty');
  };

  const handleBcvBlur = () => {
    if (tasaBcv) {
      saveBcvToFirebase(tasaBcv);
    }
  };

  // --- LÓGICA DE HERRAMIENTAS EXTRAS ---
  
  // A) Contador de Billetes
  const calculateTotalCash = () => {
    return Object.entries(bills).reduce((total, [denom, qty]) => {
      const quantity = parseInt(qty) || 0;
      return total + (parseInt(denom) * quantity);
    }, 0);
  };

  const handleTransferCash = () => {
    const total = calculateTotalCash();
    setAmount1(total.toString());
    setIsCounterOpen(false); 
  };

  const handleBillChange = (denom, value) => {
    if (value === '' || /^\d+$/.test(value)) {
      setBills(prev => ({ ...prev, [denom]: value }));
    }
  };

  // B) Sumador de Puntos de Venta (Alta Precisión Decimal)
  const calculateTotalPos = () => {
    const banescoCents = Math.round((parseFloat(posBanesco) || 0) * 100);
    const venezuelaCents = Math.round((parseFloat(posVenezuela) || 0) * 100);
    const total = (banescoCents + venezuelaCents) / 100;
    return total.toFixed(2);
  };

  const handleTransferPos = () => {
    const total = calculateTotalPos();
    setAmount1(total.toString()); // AHORA SE ENVÍA A AMOUNT1 (Monto Físico)
    setIsPosOpen(false);
  };

  // --- NÚCLEO MATEMÁTICO INTACTO (Solo precios actualizados) ---

  const generateTransactions = (breakdown, coins) => {
    let pool = [];
    
    if (breakdown['19L'] > 0) {
      for (let i = 0; i < breakdown['19L']; i++) {
        // PRECIO ACTUALIZADO: 230 Bs
        pool.push({ id: '19L', name: 'Botellón 19L', price: 23000 }); 
      }
    }
    
    coins.forEach(coin => {
      if (breakdown[coin.id] > 0) {
        for (let i = 0; i < breakdown[coin.id]; i++) {
          pool.push({ id: coin.id, name: coin.name, price: coin.price });
        }
      }
    });

    for (let i = pool.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [pool[i], pool[j]] = [pool[j], pool[i]];
    }

    let transactions = [];
    let clientCounter = 1;

    while (pool.length > 0) {
      const itemsToTake = Math.min(Math.floor(Math.random() * 5) + 1, pool.length);
      const currentClientItems = pool.splice(0, itemsToTake);
      
      let receiptBreakdown = {};
      let receiptTotal = 0;
      
      currentClientItems.forEach(item => {
        if (!receiptBreakdown[item.id]) {
          receiptBreakdown[item.id] = { name: item.name, qty: 0, pricePerUnit: item.price };
        }
        receiptBreakdown[item.id].qty++;
        receiptTotal += item.price;
      });

      transactions.push({
        id: `TXN-${String(clientCounter).padStart(3, '0')}`,
        items: receiptBreakdown,
        total: receiptTotal
      });
      
      clientCounter++;
    }

    return transactions;
  };

  const handleCalculate = () => {
    setIsCalculating(true);
    setExpandedTransactions(false);
    
    setTimeout(() => {
      const val1 = parseFloat(amount1) || 0;
      const val2 = parseFloat(amount2) || 0;
      const bcv = parseFloat(tasaBcv) || 0;

      const missing = Math.abs(val1 - val2);
      
      if (missing === 0) {
        setResultado({ zero: true, missingBase: 0 });
        setIsCalculating(false);
        return;
      }

      const targetCents = Math.round(missing * 100);
      const hasDecimals = targetCents % 100 !== 0;

      // PRECIOS ACTUALIZADOS
      const coins = [
        { id: '12L', name: 'Botellón 12L', price: 18000 }, // 180 Bs
        { id: '8L', name: 'Botellón 8L', price: 9000 },    // 90 Bs
        { id: '5L', name: 'Botellón 5L', price: 8000 }     // 80 Bs
      ];
      
      let capsUsed = false;
      if (hasDecimals && bcv > 0) {
        const capPriceCents = Math.round(0.15 * bcv * 100);
        if (capPriceCents > 0) {
          coins.push({ id: 'Cap', name: 'Tapa de Silicón', price: capPriceCents });
          capsUsed = true;
        }
      }

      // PRECIO ACTUALIZADO: 230 Bs
      const price19L = 23000;
      const max19L = Math.floor(targetCents / price19L);
      const min19L = Math.max(0, max19L - 2); 

      let best19L = 0;
      let minRemainder = Infinity;
      let bestBreakdown = {};
      let bestR = 0;

      for (let q19 = max19L; q19 >= min19L; q19--) {
        let R = targetCents - (q19 * price19L);
        
        let dp = new Int32Array(R + 1).fill(0);
        let choice = new Int32Array(R + 1).fill(-1);
        let itemCounts = new Int32Array(R + 1).fill(0);

        for (let i = 0; i <= R; i++) {
          for (let j = 0; j < coins.length; j++) {
            let p = coins[j].price;
            if (i >= p) {
              let prev = i - p;
              let newSum = dp[prev] + p;
              let newCount = itemCounts[prev] + 1;

              if (newSum > dp[i]) {
                dp[i] = newSum;
                choice[i] = j;
                itemCounts[i] = newCount;
              } else if (newSum === dp[i]) {
                if (newCount < itemCounts[i]) {
                  choice[i] = j;
                  itemCounts[i] = newCount;
                }
              }
            }
          }
        }

        let achievedDP = dp[R];
        let currentRemainder = R - achievedDP;

        if (currentRemainder < minRemainder) {
          minRemainder = currentRemainder;
          best19L = q19;
          bestR = R;
          
          bestBreakdown = {};
          coins.forEach(c => bestBreakdown[c.id] = 0);
          
          let curr = R;
          while (curr > 0 && choice[curr] !== -1) {
            let coinIdx = choice[curr];
            let p = coins[coinIdx].price;
            bestBreakdown[coins[coinIdx].id]++;
            curr = curr - p;
          }
        }

        if (minRemainder === 0) break;
      }

      const finalBreakdown = { '19L': best19L, ...bestBreakdown };
      
      let transactions = [];
      const totalItems = Object.values(finalBreakdown).reduce((a, b) => a + b, 0);
      if (totalItems > 0) {
        transactions = generateTransactions(finalBreakdown, coins);
      }

      setResultado({
        zero: false,
        missingBase: missing,
        targetCents,
        achievedCents: targetCents - minRemainder,
        remainderCents: minRemainder,
        breakdown: finalBreakdown,
        transactions: transactions,
        totalItems: totalItems,
        coins,
        hasDecimals,
        capsUsed
      });
      setIsCalculating(false);
    }, 100); 
  };

  return (
    <div className="min-h-screen bg-slate-50 p-4 md:p-8 font-sans text-slate-800">
      <div className="max-w-6xl mx-auto space-y-6">
        
        {/* Header */}
        <div className="bg-blue-800 rounded-2xl p-6 text-white shadow-lg flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
          <div>
            <h1 className="text-2xl md:text-3xl font-bold flex items-center gap-2">
              <Droplet className="w-8 h-8 text-blue-300" />
              Cuadre Perfecto 19L
            </h1>
            <p className="text-blue-200 mt-1 opacity-90 flex items-center gap-1">
              <ShieldCheck className="w-4 h-4" /> Distribución Creíble por Clientes
            </p>
          </div>
        </div>

        <div className="grid lg:grid-cols-12 gap-6">
          
          {/* Columna Izquierda: Controles e Inputs */}
          <div className="lg:col-span-4 space-y-6">
            <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
              <h2 className="text-lg font-semibold mb-4 text-slate-700 flex items-center gap-2">
                <Calculator className="w-5 h-5 text-blue-600" />
                Ingreso de Datos
              </h2>
              
              <div className="space-y-4">
                {/* 1. Herramientas Auxiliares (Puntos y Billetes) y Monto Físico */}
                <div>
                  <div className="flex justify-between items-center mb-2">
                    <label className="block text-sm font-medium text-slate-600">Monto en Caja Físico</label>
                    <div className="flex gap-2">
                      <button 
                        onClick={() => {
                          setIsPosOpen(!isPosOpen);
                          if(isCounterOpen) setIsCounterOpen(false);
                        }}
                        className={`text-xs font-semibold flex items-center gap-1 transition-colors px-2 py-1 rounded-md ${isPosOpen ? 'bg-blue-600 text-white' : 'bg-blue-50 text-blue-600 hover:text-blue-800'}`}
                      >
                        <CreditCard className="w-3 h-3" />
                        Puntos
                      </button>
                      <button 
                        onClick={() => {
                          setIsCounterOpen(!isCounterOpen);
                          if(isPosOpen) setIsPosOpen(false);
                        }}
                        className={`text-xs font-semibold flex items-center gap-1 transition-colors px-2 py-1 rounded-md ${isCounterOpen ? 'bg-blue-600 text-white' : 'bg-blue-50 text-blue-600 hover:text-blue-800'}`}
                      >
                        <Wallet className="w-3 h-3" />
                        Efectivo
                      </button>
                    </div>
                  </div>

                  {/* Panel de Puntos de Venta */}
                  {isPosOpen && (
                    <div className="mb-3 p-4 bg-slate-50 border border-slate-200 rounded-xl animate-in fade-in slide-in-from-top-2 duration-200">
                      <p className="text-xs text-slate-500 font-medium mb-3 uppercase tracking-wider text-center">Sumar Puntos de Venta</p>
                      <div className="grid grid-cols-1 gap-3">
                        <div className="flex items-center gap-2">
                          <span className="text-sm font-bold text-slate-600 w-24 text-right border-r border-slate-300 pr-2">Banesco</span>
                          <input 
                            type="number" 
                            step="0.01"
                            value={posBanesco}
                            onChange={(e) => setPosBanesco(e.target.value)}
                            placeholder="0.00"
                            className="w-full py-1 px-2 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm"
                          />
                        </div>
                        <div className="flex items-center gap-2">
                          <span className="text-sm font-bold text-slate-600 w-24 text-right border-r border-slate-300 pr-2">Venezuela</span>
                          <input 
                            type="number" 
                            step="0.01"
                            value={posVenezuela}
                            onChange={(e) => setPosVenezuela(e.target.value)}
                            placeholder="0.00"
                            className="w-full py-1 px-2 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm"
                          />
                        </div>
                      </div>
                      
                      <div className="mt-4 pt-3 border-t border-slate-200 flex items-center justify-between">
                        <div className="font-bold text-slate-700">
                          Total: <span className="text-blue-700">Bs. {calculateTotalPos()}</span>
                        </div>
                        <button 
                          onClick={handleTransferPos}
                          disabled={parseFloat(calculateTotalPos()) === 0}
                          className="bg-blue-600 hover:bg-blue-700 disabled:bg-slate-300 text-white text-xs font-bold px-3 py-2 rounded-lg flex items-center gap-1 transition-colors"
                        >
                          <ArrowDownCircle className="w-4 h-4" /> Usar total
                        </button>
                      </div>
                    </div>
                  )}
                  
                  {/* Panel del Contador de Billetes */}
                  {isCounterOpen && (
                    <div className="mb-3 p-4 bg-slate-50 border border-slate-200 rounded-xl animate-in fade-in slide-in-from-top-2 duration-200">
                      <p className="text-xs text-slate-500 font-medium mb-3 uppercase tracking-wider text-center">Cantidad de billetes</p>
                      <div className="grid grid-cols-2 gap-3">
                        {[500, 200, 100, 50, 20, 10].map(denom => (
                          <div key={denom} className="flex items-center gap-2">
                            <span className="text-sm font-bold text-slate-600 w-12 text-right border-r border-slate-300 pr-2">Bs.{denom}</span>
                            <input 
                              type="number" 
                              min="0"
                              value={bills[denom]}
                              onChange={(e) => handleBillChange(denom, e.target.value)}
                              placeholder="0"
                              className="w-full text-center py-1 px-2 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-sm"
                            />
                          </div>
                        ))}
                      </div>
                      
                      <div className="mt-4 pt-3 border-t border-slate-200 flex items-center justify-between">
                        <div className="font-bold text-slate-700">
                          Total: <span className="text-blue-700">Bs. {calculateTotalCash()}</span>
                        </div>
                        <button 
                          onClick={handleTransferCash}
                          disabled={calculateTotalCash() === 0}
                          className="bg-blue-600 hover:bg-blue-700 disabled:bg-slate-300 text-white text-xs font-bold px-3 py-2 rounded-lg flex items-center gap-1 transition-colors"
                        >
                          <ArrowDownCircle className="w-4 h-4" /> Usar total
                        </button>
                      </div>
                    </div>
                  )}

                  {/* Input Principal Monto Físico */}
                  <div className="relative">
                    <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 font-medium">Bs.</span>
                    <input 
                      type="number" 
                      value={amount1}
                      onChange={(e) => setAmount1(e.target.value)}
                      placeholder="0.00"
                      className="w-full pl-10 pr-4 py-2 border border-slate-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all"
                    />
                  </div>
                </div>

                {/* 2. Monto del Sistema (Limpio) */}
                <div>
                  <label className="block text-sm font-medium text-slate-600 mb-1">Monto del Sistema</label>
                  <div className="relative">
                    <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 font-medium">Bs.</span>
                    <input 
                      type="number" 
                      value={amount2}
                      onChange={(e) => setAmount2(e.target.value)}
                      placeholder="0.00"
                      className="w-full pl-10 pr-4 py-2 border border-slate-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all"
                    />
                  </div>
                </div>

                {/* 3. Tasa BCV */}
                <div className="pt-3 border-t border-slate-100 mt-2">
                  <div className="flex justify-between items-center mb-1">
                    <label className="block text-sm font-medium text-slate-600">Tasa BCV del Día</label>
                    {/* Indicador de estado BCV */}
                    {bcvStatus === 'current' && (
                      <span className="flex items-center gap-1 text-[10px] font-bold bg-green-100 text-green-700 px-2 py-0.5 rounded-full uppercase tracking-wider">
                        <Check className="w-3 h-3" /> Al día
                      </span>
                    )}
                    {bcvStatus === 'old' && (
                      <span className="flex items-center gap-1 text-[10px] font-bold bg-orange-100 text-orange-700 px-2 py-0.5 rounded-full uppercase tracking-wider animate-pulse">
                        <Clock className="w-3 h-3" /> Desactualizada
                      </span>
                    )}
                  </div>
                  
                  <div className="relative">
                    <span className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 font-medium">$</span>
                    <input 
                      type="text" 
                      inputMode="decimal"
                      value={tasaBcv}
                      onChange={handleBcvChange}
                      onBlur={handleBcvBlur}
                      placeholder="Ej. 36.50"
                      className={`w-full pl-10 pr-4 py-2 border-2 rounded-xl outline-none transition-all ${
                        bcvStatus === 'old' 
                          ? 'border-orange-300 focus:ring-orange-500 focus:border-orange-500 bg-orange-50' 
                          : 'border-slate-300 focus:ring-emerald-500 focus:border-emerald-500'
                      }`}
                    />
                  </div>
                  {bcvStatus === 'old' && (
                    <p className="text-xs text-orange-600 mt-1 flex items-center gap-1">
                       <AlertCircle className="w-3 h-3"/> Cuidado, esta tasa es de días anteriores.
                    </p>
                  )}
                </div>

                <button 
                  onClick={handleCalculate}
                  disabled={isCalculating || (!amount1 && !amount2)}
                  className="w-full mt-4 bg-blue-700 hover:bg-blue-800 text-white font-semibold py-3 px-4 rounded-xl shadow-md transition-colors disabled:opacity-50 disabled:cursor-not-allowed flex items-center justify-center gap-2"
                >
                  {isCalculating ? 'Procesando...' : 'Generar Maquillaje'}
                  {!isCalculating && <ArrowRight className="w-5 h-5" />}
                </button>
              </div>
            </div>

             {/* Referencia de Precios ACTUALIZADOS */}
             <div className="bg-slate-100 p-5 rounded-2xl border border-slate-200">
              <h3 className="text-sm font-semibold text-slate-700 mb-3 flex items-center gap-2">
                <Info className="w-4 h-4 text-blue-600" />
                Precios Activos
              </h3>
              <ul className="space-y-2 text-sm text-slate-600">
                <li className="flex justify-between font-bold text-blue-800"><span>⭐ Botellón 19L</span> <span>Bs. 230.00</span></li>
                <li className="flex justify-between"><span>Botellón 12L</span> <span>Bs. 180.00</span></li>
                <li className="flex justify-between"><span>Botellón 8L</span> <span>Bs. 90.00</span></li>
                <li className="flex justify-between"><span>Botellón 5L</span> <span>Bs. 80.00</span></li>
                <li className="flex justify-between text-emerald-700 border-t border-slate-200 pt-2 mt-2">
                  <span>Tapa de Silicón ($0.15)</span> 
                  <span className="font-medium">{tasaBcv ? `Bs. ${(0.15 * parseFloat(tasaBcv)).toFixed(2)}` : 'Requiere BCV'}</span>
                </li>
              </ul>
            </div>
          </div>

          {/* Columna Derecha: Resultados */}
          <div className="lg:col-span-8">
            {resultado ? (
              <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 h-full flex flex-col">
                
                {resultado.zero && resultado.missingBase === 0 ? (
                  <div className="flex-1 flex flex-col items-center justify-center text-center p-8 bg-green-50 rounded-xl border border-green-100">
                    <CheckCircle className="w-20 h-20 text-green-500 mb-4" />
                    <h2 className="text-2xl font-bold text-green-800">Cuadre Perfecto</h2>
                    <p className="text-green-600 mt-2">No necesitas maquillar nada, la caja está en cero.</p>
                  </div>
                ) : (
                  <>
                    <div className="mb-6 pb-6 border-b border-slate-200 flex flex-col sm:flex-row sm:items-end justify-between gap-4">
                      <div>
                        <p className="text-sm text-slate-500 font-bold uppercase tracking-wider mb-1">Diferencia Total a Justificar</p>
                        <h2 className="text-4xl font-black text-slate-800">
                          Bs. {resultado.missingBase.toFixed(2)}
                        </h2>
                      </div>
                      
                      <div className={`p-3 rounded-xl flex items-center gap-3 border-2 ${resultado.remainderCents === 0 ? 'bg-green-50 border-green-200 text-green-700' : 'bg-amber-50 border-amber-200 text-amber-700'}`}>
                        {resultado.remainderCents === 0 ? <CheckCircle className="w-6 h-6" /> : <AlertCircle className="w-6 h-6" />}
                        <div>
                           <p className="text-xs font-bold uppercase">{resultado.remainderCents === 0 ? 'Cuadre a Cero' : 'Sobrante'}</p>
                           {resultado.remainderCents !== 0 && (
                             <p className="font-black text-lg">Bs. {(resultado.remainderCents / 100).toFixed(2)}</p>
                           )}
                        </div>
                      </div>
                    </div>

                    <div className="flex-1">
                      
                      {/* Vista Resumen (Totales) */}
                      <div className="mb-6 bg-slate-50 p-4 rounded-xl border border-slate-200">
                        <h3 className="text-sm font-bold text-slate-700 mb-3 uppercase tracking-wider">Resumen de Artículos</h3>
                        <div className="flex flex-wrap gap-2">
                           {resultado.breakdown['19L'] > 0 && (
                             <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-bold border border-blue-200">
                               {resultado.breakdown['19L']}x 19L
                             </span>
                           )}
                           {resultado.coins?.map(coin => {
                             if (resultado.breakdown[coin.id] > 0) {
                               return (
                                 <span key={coin.id} className="bg-slate-200 text-slate-700 px-3 py-1 rounded-full text-sm font-bold border border-slate-300">
                                   {resultado.breakdown[coin.id]}x {coin.name.replace('Botellón ', '').replace('Tapa de Silicón', 'Tapas')}
                                 </span>
                               )
                             }
                             return null;
                           })}
                        </div>
                      </div>

                      {/* Vista Detallada por Clientes (Transacciones) */}
                      <div className="border border-slate-200 rounded-xl overflow-hidden">
                        <button 
                          onClick={() => setExpandedTransactions(!expandedTransactions)}
                          className="w-full bg-white p-4 flex items-center justify-between hover:bg-slate-50 transition-colors"
                        >
                          <div className="flex items-center gap-3">
                            <Users className="w-5 h-5 text-blue-600" />
                            <div className="text-left">
                              <h3 className="font-bold text-slate-800 text-lg">Facturación Sugerida</h3>
                              <p className="text-sm text-slate-500">{resultado.transactions?.length || 0} recibos generados (Max 5 items c/u)</p>
                            </div>
                          </div>
                          {expandedTransactions ? <ChevronUp className="w-5 h-5 text-slate-400" /> : <ChevronDown className="w-5 h-5 text-slate-400" />}
                        </button>

                        {expandedTransactions && (
                          <div className="bg-slate-50 border-t border-slate-200 p-4 space-y-4 max-h-[500px] overflow-y-auto">
                            {resultado.transactions?.length > 0 ? (
                              resultado.transactions.map((txn, index) => (
                                <div key={txn.id} className="bg-white border border-slate-200 rounded-lg shadow-sm overflow-hidden">
                                  {/* Header del Ticket */}
                                  <div className="bg-slate-100 px-4 py-2 flex justify-between items-center border-b border-slate-200">
                                    <span className="font-bold text-slate-600 text-sm flex items-center gap-1">
                                      <Receipt className="w-4 h-4" /> Cliente #{index + 1}
                                    </span>
                                    <span className="font-black text-blue-800">Bs. {(txn.total / 100).toFixed(2)}</span>
                                  </div>
                                  
                                  {/* Cuerpo del Ticket */}
                                  <div className="p-4 space-y-2">
                                    {Object.entries(txn.items).map(([itemId, itemData]) => (
                                      <div key={itemId} className="flex justify-between items-center text-sm">
                                        <div className="flex items-center gap-2">
                                          <span className="font-bold text-slate-700 min-w-[20px]">{itemData.qty}x</span>
                                          <span className={`${itemId === '19L' ? 'text-blue-700 font-semibold' : 'text-slate-600'}`}>
                                            {itemData.name}
                                          </span>
                                        </div>
                                        <span className="text-slate-500">
                                          Bs. {((itemData.qty * itemData.pricePerUnit) / 100).toFixed(2)}
                                        </span>
                                      </div>
                                    ))}
                                  </div>
                                </div>
                              ))
                            ) : (
                              <p className="text-center text-slate-500 py-4">No hay transacciones para mostrar.</p>
                            )}
                          </div>
                        )}
                      </div>
                      {!expandedTransactions && resultado.transactions?.length > 0 && (
                        <p className="text-center text-sm text-blue-600 mt-2 animate-pulse cursor-pointer" onClick={() => setExpandedTransactions(true)}>
                          Toca aquí para ver cómo anotar en el cuaderno 👇
                        </p>
                      )}

                    </div>
                  </>
                )}
              </div>
            ) : (
              <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 h-full flex flex-col items-center justify-center text-center">
                <Receipt className="w-20 h-20 text-blue-100 mb-4" />
                <h3 className="text-xl font-bold text-slate-700">Listo para cuadrar</h3>
                <p className="text-slate-500 mt-2 max-w-md">
                  Ingresa los montos. Puedes usar las herramientas para sumar tus billetes y tus puntos de venta fácilmente. El sistema calculará la mejor combinación para tu cuaderno.
                </p>
              </div>
            )}
          </div>
        </div>
      </div>
    </div>
  );
}

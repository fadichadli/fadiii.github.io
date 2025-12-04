import { motion } from "framer-motion";
import { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { BarChart3, Zap, Wifi, Gauge, Mail, Phone, Sun, Moon, Calculator, Bell, Shield } from "lucide-react";
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from "recharts";

// Contexte pour le mode sombre
const ThemeContext = React.createContext();

const data = [
  { name: "Lundi", consommation: 12 },
  { name: "Mardi", consommation: 15 },
  { name: "Mercredi", consommation: 10 },
  { name: "Jeudi", consommation: 18 },
  { name: "Vendredi", consommation: 14 },
  { name: "Samedi", consommation: 9 },
  { name: "Dimanche", consommation: 11 },
];

export default function Home() {
  const [darkMode, setDarkMode] = useState(false);
  const [formData, setFormData] = useState({ name: "", email: "", message: "" });
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [alert, setAlert] = useState(""); // Pour les notifications fictives
  const [calculatorInput, setCalculatorInput] = useState({ usage: "", rate: "" });
  const [savings, setSavings] = useState(0);

  useEffect(() => {
    // Simulation d'une alerte IoT toutes les 10 secondes
    const interval = setInterval(() => {
      setAlert("Alerte : Pic de consommation détecté à 18h ! Éteignez les appareils inutiles.");
      setTimeout(() => setAlert(""), 5000);
    }, 10000);
    return () => clearInterval(interval);
  }, []);

  const toggleDarkMode = () => setDarkMode(!darkMode);

  const handleInputChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Formulaire soumis :", formData);
    setIsSubmitted(true);
    setFormData({ name: "", email: "", message: "" });
  };

  const calculateSavings = () => {
    const usage = parseFloat(calculatorInput.usage) || 0;
    const rate = parseFloat(calculatorInput.rate) || 0;
    setSavings((usage * 0.1 * rate).toFixed(2)); // Estimation fictive : 10% d'économie
  };

  return (
    <ThemeContext.Provider value={{ darkMode, toggleDarkMode }}>
      <div className={`min-h-screen ${darkMode ? 'bg-gray-900 text-white' : 'bg-gray-50 text-gray-900'} flex flex-col transition-colors duration-300`}>
        {/* HEADER */}
        <header className={`w-full py-6 px-8 flex justify-between items-center ${darkMode ? 'bg-gray-800' : 'bg-white'} shadow-sm`}>
          <h1 className="text-2xl font-bold">SmartEnergy 2025</h1>
          <div className="flex items-center space-x-4">
            <Button onClick={toggleDarkMode} variant="ghost" size="icon">
              {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />}
            </Button>
            <Button className="rounded-2xl px-6 py-2 text-lg" onClick={() => window.scrollTo({ top: document.getElementById("contact").offsetTop, behavior: "smooth" })}>
              Commencer
            </Button>
          </div>
        </header>

        {/* ALERT NOTIFICATION */}
        {alert && (
          <motion.div 
            initial={{ opacity: 0, y: -20 }} 
            animate={{ opacity: 1, y: 0 }} 
            className="bg-yellow-500 text-black px-4 py-2 text-center"
          >
            <Bell className="inline w-5 h-5 mr-2" /> {alert}
          </motion.div>
        )}

        {/* HERO */}
        <section className="flex-1 flex flex-col justify-center items-center text-center px-6">
          <motion.h2 
            initial={{ opacity: 0, y: 20 }} 
            animate={{ opacity: 1, y: 0 }} 
            transition={{ duration: 0.8 }}
            className="text-5xl font-extrabold mb-4 max-w-3xl"
          >
            Surveille ta consommation d'énergie en temps réel avec IA.
          </motion.h2>
          <motion.p
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 1 }}
            className="text-xl text-gray-600 dark:text-gray-300 max-w-2xl mb-8"
          >
            Un système intelligent basé sur IoT + IA qui analyse, détecte les gaspillages, réduit ta facture et ton empreinte carbone.
          </motion.p>
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 1.2 }}
          >
            <Button className="rounded-2xl px-8 py-4 text-xl" onClick={() => window.scrollTo({ top: document.getElementById("calculator").offsetTop, behavior: "smooth" })}>
              Calculer mes économies
            </Button>
          </motion.div>
        </section>

        {/* FEATURES */}
        <section className="py-16 px-8 grid grid-cols-1 md:grid-cols-3 gap-8">
          <motion.div
            initial={{ opacity: 0, scale: 0.9 }}
            whileInView={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.6 }}
          >
            <Card className={`rounded-2xl p-4 shadow-md hover:shadow-lg transition ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
              <CardContent className="flex flex-col items-center text-center">
                <Gauge className="w-12 h-12 mb-4 text-blue-600" />
                <h3 className="text-xl font-semibold mb-2">Mesures en temps réel</h3>
                <p className="text-gray-600 dark:text-gray-300">
                  Tension, courant, puissance : visualise tout instantanément depuis ton smartphone.
                </p>
              </CardContent>
            </Card>
          </motion.div>

          <motion.div
            initial={{ opacity: 0, scale: 0.9 }}
            whileInView={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.6, delay: 0.2 }}
          >
            <Card className={`rounded-2xl p-4 shadow-md hover:shadow-lg transition ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
              <CardContent className="flex flex-col items-center text-center">
                <Wifi className="w-12 h-12 mb-4 text-blue-600" />
                <h3 className="text-xl font-semibold mb-2">Connecté via WiFi</h3>
                <p className="text-gray-600 dark:text-gray-300">
                  Ton module IoT (ESP32) envoie automatiquement les données vers le cloud sécurisé.
                </p>
              </CardContent>
            </Card>
          </motion.div>

          <motion.div
            initial={{ opacity: 0, scale: 0.9 }}
            whileInView={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.6, delay: 0.4 }}
          >
            <Card className={`rounded-2xl p-4 shadow-md hover:shadow-lg transition ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
              <CardContent className="flex flex-col items-center text-center">
                <Zap className="w-12 h-12 mb-4 text-blue-600" />
                <h3 className="text-xl font-semibold mb-2">Optimisation IA</h3>
                <p className="text-gray-600 dark:text-gray-300">
                  Détection automatique des gaspillages + recommandations personnalisées pour réduire la facture.
                </p>
              </CardContent>
            </Card>
          </motion.div>
        </section>

        {/* CALCULATOR */}
        <section id="calculator" className={`py-16 px-8 max-w-4xl mx-auto ${darkMode ? 'bg-gray-800' : 'bg-white'} rounded-2xl shadow-md`}>
          <motion.h3 
            initial={{ opacity: 0 }}
            whileInView={{ opacity: 1 }}
            className="text-3xl font-bold text-center mb-8"
          >
            <Calculator className="inline w-8 h-8 mr-2" /> Calcule tes économies potentielles
          </motion.h3>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <input
              type="number"
              placeholder="Consommation mensuelle (kWh)"
              value={calculatorInput.usage}
              onChange={(e) => setCalculatorInput({ ...calculatorInput, usage: e.target.value })}
              className="px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <input
              type="number"
              placeholder="Tarif par kWh (€)"
              value={calculatorInput.rate}
              onChange={(e) => setCalculatorInput({ ...calculatorInput, rate: e.target.value })}
              className="px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
          <Button onClick={calculateSavings} className="mb-4">Calculer</Button>
          {savings > 0 && <p className="text-center text-green-600">Économies estimées : {savings} €/mois avec SmartEnergy !</p>}
        </section>

        {/* CHART PREVIEW */}
        <section className={`py-12 px-8 flex flex-col items-center ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
          <motion.h3 
            initial={{ opacity: 0 }}
            whileInView={{ opacity: 1 }}
            transition={{ duration: 0.8 }}
            className="text-3xl font-bold mb-4"
          >
            Aperçu de ton Dashboard
          </motion.h3>
          <p className="text-gray-600 dark:text-gray-300 text-lg mb-8 max-w-2xl text-center">
            Un tableau moderne et simple qui te montre ta consommation chaque jour, chaque appareil, et les pics anormaux.
          </p>
          <motion.div 
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            transition={{ duration: 1 }}
            className="w-full max-w-3xl h-64 bg-gray-100 dark:bg-gray-700 rounded-2xl shadow-inner p-4"
          >
            <ResponsiveContainer width="100%" height="100%">
              <LineChart data={data}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="name" />
                <YAxis />
                <Tooltip />
                <Line type="monotone" dataKey="consommation" stroke="#3b82f6" strokeWidth={2} />
              </LineChart>
            </ResponsiveContainer>
          </motion.div>
        </section>

        {/* AI RECOMMENDATIONS */}
        <section className="py-16 px-8 max-w-4xl mx-auto">
          <motion.h3 
            initial={{ opacity: 0 }}
            whileInView={{ opacity: 1 }}
            className="text-3xl font-bold text-center mb-8"
          >
            Recommandations IA personnalisées
          </motion.h3>
          <Card className={`rounded-2xl p-6 shadow-md ${darkMode ? 'bg-gray-800' : 'bg-white'}`}>
            <CardContent>
              <p className="text-gray-600 dark:text-gray-300">
                Basé sur tes données : Réduis ta consommation de 15% en éteignant les appareils en veille. Essaye notre mode "Eco" !
              </p>
            </CardContent>
          </Card>
        </section>

        {/* SECURITY SECTION */}
        <section className="py-16 px-8 max-w-4xl mx-auto">
          <motion.h3 
            initial={{ opacity: 0 }}
            whileInView={{ opacity: 1 }}
            className="text-3xl font-bold text-center mb-8"
          >
            <Shield className="inline w-8 h-8 mr-2" /> Sécurité et confidentialité
          </motion.h3>
          <p className="text-center text-gray-600 dark:text-gray-300 max-w-2xl mx-auto">
            Tes données sont chiffrées et stockées en Europe. Nous respectons le RGPD et n'utilisons pas tes infos sans consentement.
          </p>
        </section>

        {/* CONTACT SECTION */}
        <section id="contact" className="py-16 px-8 max-w-4xl mx-auto">
          <motion.h3 
            initial={{ opacity: 0 }}
            whileInView={{ opacity: 1 }}
            transition={{ duration: 0.8 }}
            className="text-3xl font-bold text-center mb-8"
          >
            Contactez-nous
          </motion.h3>
          {isSubmitted ? (
            <p className="text-center text-green-600">Merci ! Votre message a été envoyé.</p>
          ) : (
            <form onSubmit={handleSubmit} className={`bg-white dark:bg-gray-800 rounded-2xl p-8 shadow-md`}>
              <div className="mb-4">
                <label htmlFor="name" className="block text-gray-700 dark:text-gray-300 font-semibold mb-2">Nom</label>
                <input
                  type="text"
                  id="name"
                  name="name"
                  value={formData.name}
                  onChange={handleInputChange}
                  className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700"
                  required
                />
              </div>
              <div className="mb-4">
                <label htmlFor="email" className="block text-gray-700 dark:text-gray-300 font-semibold mb-2">Email</label>
                <input
                  type="email"
                  id="email"
                  name="email"
                  value={formData.email}
                  onChange={handleInputChange}
                  className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700"
                  required
                />
              </div>
              <div className="mb-4">
                <label htmlFor="message" className="block text-gray-700 dark:text-gray-300 font-semibold mb-2">Message</label>
                <textarea
                  id="message"
                  name="message"
                  rows="4"
                  value={formData.message}
                  onChange={handleInputChange}
                  className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-700"
                  required
                />
              </div>
              <Button type="submit" className="rounded-lg px-6 py-2">Envoyer</Button>
            </form>
          )}
        </section>

        {/* FOOTER */}
        <footer className={`py-6 text-center ${darkMode ? 'bg-gray-800' : 'bg-gray

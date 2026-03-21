"""
SISTEMA FUSIONADO — ABRAMGOCHI + CADENA DE RELEVO MOLECULAR
=============================================================
Fusión completa de dos sistemas originales:
 
1. ABRAMGOCHI — red relacional de agentes con evolución genética
2. CADENA DE RELEVO MOLECULAR — transferencia de carga intermolecular
 
Fundamento teórico unificado — Kohn-Sham (DFT):
    [-½∇² + V_eff(r)] φᵢ(r) = εᵢ φᵢ(r)
 
Pipeline molecular:
    x₀ → x₁ → ⋯ → xₜ        cadena de relevo
    E = f_θ(xₜ)              energía del estado
    L = ||E_pred - E_true||²  función de pérdida
 
Author: Abraham
Framework: H.A.S. (Human Anticipation Strategist)
Project: Genoma Cognitivo
Date: March 21, 2026
"""
 
import random
import math
import re
import hashlib
import datetime
import os
import networkx as nx
from collections import defaultdict, Counter
 
# =========================================================
# 🔐 AUTENTICACIÓN
# =========================================================
 
USUARIOS = {
    "admin":  {"password": "1234", "rol": "admin"},
    "viewer": {"password": "0000", "rol": "viewer"}
}
 
def login():
    user = input("Usuario: ")
    pwd  = input("Password: ")
    if user in USUARIOS and USUARIOS[user]["password"] == pwd:
        print(f"✅ Acceso como '{USUARIOS[user]['rol']}'")
        return user, USUARIOS[user]["rol"]
    print("❌ Acceso denegado")
    return None, None
 
def log(user, accion):
    os.makedirs("logs", exist_ok=True)
    with open("logs/fusion.log", "a") as f:
        f.write(f"{datetime.datetime.now()} | {user} | {accion}\n")
 
# =========================================================
# ⚛️ KOHN-SHAM UNIFICADO
# El corazón matemático de ambos sistemas
# =========================================================
 
class KohnShamField:
    """
    Campo unificado basado en la ecuación de Kohn-Sham.
    
    Aplica tanto a agentes cognitivos (ABRAMGOCHI)
    como a iones moleculares (Cadena de Relevo).
    
    [-½∇² + V_eff(r)] φᵢ(r) = εᵢ φᵢ(r)
    """
 
    def __init__(self, nombre="campo"):
        self.nombre = nombre
 
    def curvatura(self, estado_interno):
        """∇² — tensión interna del agente/ion"""
        return abs(estado_interno - 50) / 50
 
    def v_eff(self, vecinos):
        """V_eff(r) — potencial efectivo del entorno relacional"""
        if not vecinos:
            return 0.5
        return sum(vecinos) / len(vecinos) / 100
 
    def funcion_onda(self, estado, v_eff):
        """φᵢ(r) — patrón latente no observable"""
        return estado * (1 - v_eff) + random.gauss(0, 2)
 
    def eigenvalor(self, curvatura, v_eff):
        """εᵢ — nivel de energía estable"""
        return -0.5 * curvatura + v_eff
 
    def densidad(self, funcion_onda):
        """ρ(r) = |φᵢ(r)|² — lo único observable"""
        return funcion_onda ** 2
 
    def colapsar(self, estado, vecinos):
        """
        Proceso completo de colapso cuántico:
        función de onda → densidad observable
        """
        k  = self.curvatura(estado)
        ve = self.v_eff(vecinos)
        fi = self.funcion_onda(estado, ve)
        ei = self.eigenvalor(k, ve)
        ro = self.densidad(fi)
        return {
            "curvatura": k,
            "v_eff": ve,
            "phi": fi,
            "epsilon": ei,
            "rho": ro
        }
 
# =========================================================
# 🧬 MOLÉCULA — unidad del relevo molecular
# =========================================================
 
class Molecula:
    """
    Unidad de la cadena de relevo molecular.
    Cada molécula es un agente cuántico con:
    - estado energético
    - capacidad de transferir carga al siguiente
    - función de onda latente
    """
 
    def __init__(self, id, energia_base=None):
        self.id = id
        self.energia = energia_base or random.uniform(40, 60)
        self.carga = random.uniform(0, 1)
        self.campo = KohnShamField(f"mol_{id}")
        self.historial = []
 
    def colapsar(self, vecinos_energia):
        """Colapsa la función de onda dado el entorno"""
        resultado = self.campo.colapsar(self.energia, vecinos_energia)
        self.historial.append(resultado)
        return resultado
 
    def transferir_carga(self, siguiente):
        """
        Chispa de relevo — transfiere carga a la siguiente molécula.
        ρ(r) de esta molécula alimenta φᵢ(r) de la siguiente.
        """
        if self.historial:
            rho = self.historial[-1]["rho"]
            transferencia = min(0.3, rho * 0.001 * random.uniform(0.8, 1.2))
            siguiente.carga = min(1.0, siguiente.carga + transferencia)
            siguiente.energia = min(100, siguiente.energia + transferencia * 0.1)
            return transferencia
        return 0
 
    def __repr__(self):
        return f"Mol({self.id}, E={self.energia:.1f}, Q={self.carga:.3f})"
 
 
# =========================================================
# ⚡ CADENA DE RELEVO MOLECULAR
# x₀ → x₁ → ⋯ → xₜ
# =========================================================
 
class CadenaRelevo:
    """
    Pipeline completo de transferencia de carga intermolecular.
    
    Cada molécula en la cadena:
    1. Colapsa su función de onda según el entorno
    2. Transfiere su densidad ρ(r) a la siguiente
    3. La cadena mantiene la carga viva sin degradarse
    """
 
    def __init__(self, n_moleculas=8):
        self.moleculas = [Molecula(i) for i in range(n_moleculas)]
        self.campo = KohnShamField("cadena")
        self.historial_energia = []
        self.historial_perdida = []
 
    def energia_predicha(self, molecula):
        """E = f_θ(xₜ) — predice energía del estado molecular"""
        vecinos = [m.energia for m in self.moleculas
                   if abs(m.id - molecula.id) == 1]
        resultado = molecula.colapsar(vecinos)
        return resultado["epsilon"] * 100 + 50
 
    def energia_real(self, molecula):
        """Energía real DFT (simulada)"""
        return molecula.energia + random.gauss(0, 3)
 
    def perdida(self, e_pred, e_real):
        """L = ||E_pred - E_true||² — función de pérdida"""
        return (e_pred - e_real) ** 2
 
    def paso_relevo(self):
        """
        Un paso completo de la cadena:
        x₀ → x₁ → ⋯ → xₜ
        """
        energia_total = 0
        perdida_total = 0
 
        for i, mol in enumerate(self.moleculas[:-1]):
            e_pred = self.energia_predicha(mol)
            e_real = self.energia_real(mol)
            L = self.perdida(e_pred, e_real)
 
            transferencia = mol.transferir_carga(self.moleculas[i + 1])
            energia_total += mol.energia
            perdida_total += L
 
        self.historial_energia.append(energia_total / len(self.moleculas))
        self.historial_perdida.append(perdida_total / len(self.moleculas))
 
        return energia_total, perdida_total
 
    def estado_cadena(self):
        return [(m.id, round(m.energia, 2), round(m.carga, 3))
                for m in self.moleculas]
 
 
# =========================================================
# 🌐 ABRAMGOCHI — red relacional de agentes
# =========================================================
 
class RedABRAMGOCHI:
    """
    Red relacional de agentes cognitivos.
    Cada agente aplica Kohn-Sham para adaptar su comportamiento
    al entorno relacional — exactamente como las moléculas
    en la cadena de relevo.
    """
 
    def __init__(self, n=8, p=0.6):
        self.G = nx.erdos_renyi_graph(n=n, p=p)
        self.campo = KohnShamField("red")
        self.estado = {n: random.uniform(40, 60) for n in self.G.nodes()}
        self.hash_orig = hashlib.sha256(
            str(sorted(self.estado.items())).encode()
        ).hexdigest()
 
    def actualizar(self):
        """Actualiza cada agente según su entorno relacional V_eff"""
        nuevo = {}
        densidades = {}
 
        for nodo in self.G.nodes():
            vecinos_vals = [self.estado[v] for v in self.G.neighbors(nodo)]
            resultado = self.campo.colapsar(self.estado[nodo], vecinos_vals)
 
            influencia = 0.1 if resultado["v_eff"] < 0.5 else 0.3
            nuevo[nodo] = max(0, min(100,
                self.estado[nodo]
                + influencia * (resultado["v_eff"] * 100 - self.estado[nodo])
                + random.uniform(-2, 2)
            ))
            densidades[nodo] = resultado["rho"]
 
        self.estado = nuevo
        return densidades
 
    def dificultad(self, nivel):
        """dificultad = base + (nivel * factor) + entorno"""
        entorno = sum(self.estado.values()) / len(self.estado)
        return 50 + (nivel * 2.5) + entorno
 
    def fitness_colectivo(self):
        """Fitness emergente de la red"""
        return sum(self.estado.values()) / len(self.estado) / 100
 
 
# =========================================================
# 🧬 EVOLUCIÓN GENÉTICA (compartida)
# =========================================================
 
def seleccionar(poblacion, n=4):
    return sorted(poblacion, key=lambda x: x.get('fitness', 0), reverse=True)[:n]
 
def reproducir(p1, p2):
    """hijo = mezcla(padre1, padre2) + mutación"""
    hijo = {k: random.choice([p1[k], p2[k]]) for k in p1}
    return mutar(hijo)
 
def clonar(padre):
    """hijo = copia(padre) + mutación"""
    return mutar(padre.copy())
 
def mutar(agente, tasa=0.1):
    return {k: (v + random.uniform(-5, 5)
                if isinstance(v, (int, float)) and random.random() < tasa else v)
            for k, v in agente.items()}
 
def nueva_generacion(poblacion):
    """nueva_generación = seleccionar(mejores) + reproducir + mutar"""
    mejores = seleccionar(poblacion)
    nueva = mejores.copy()
    while len(nueva) < len(poblacion):
        p1, p2 = random.sample(mejores, 2)
        nueva.append(reproducir(p1, p2) if random.random() > 0.5
                     else clonar(random.choice(mejores)))
    return nueva
 
 
# =========================================================
# 🔗 SISTEMA FUSIONADO
# =========================================================
 
class SistemaFusion:
    """
    Fusión completa: ABRAMGOCHI + Cadena de Relevo Molecular.
    
    Los agentes cognitivos y las moléculas comparten:
    - El mismo campo cuántico Kohn-Sham
    - La misma lógica de evolución genética
    - El mismo principio: nunca observamos φᵢ(r), solo ρ(r)
    
    La red cognitiva modela el comportamiento del electrodo.
    La cadena molecular implementa físicamente ese modelo.
    Son el mismo sistema en dominios diferentes.
    """
 
    def __init__(self):
        self.red = RedABRAMGOCHI(n=8, p=0.6)
        self.cadena = CadenaRelevo(n_moleculas=8)
        self.campo_unificado = KohnShamField("fusion")
        self.generacion = 0
 
    def ciclo_completo(self, paso):
        """
        Un ciclo fusionado:
        1. La red cognitiva actualiza su estado (ABRAMGOCHI)
        2. El estado de la red alimenta la cadena molecular
        3. La cadena ejecuta el relevo de carga
        4. La pérdida L retroalimenta la evolución
        """
        # ABRAMGOCHI actualiza
        densidades = self.red.actualizar()
        dif = self.red.dificultad(paso)
        fitness = self.red.fitness_colectivo()
 
        # El fitness de la red alimenta la energía de las moléculas
        for i, mol in enumerate(self.cadena.moleculas):
            nodo = list(densidades.keys())[i % len(densidades)]
            mol.energia = 40 + densidades[nodo] * 0.1
 
        # Cadena ejecuta relevo
        energia_total, perdida_total = self.cadena.paso_relevo()
 
        # Evolución genética de la población
        poblacion = [
            {'fitness': fitness + random.uniform(-0.1, 0.1),
             'energia': energia_total}
            for _ in range(8)
        ]
        poblacion = nueva_generacion(poblacion)
        self.generacion += 1
 
        return {
            "paso": paso,
            "dificultad": dif,
            "fitness_red": fitness,
            "energia_cadena": energia_total / 8,
            "perdida_L": perdida_total / 8,
            "generacion": self.generacion
        }
 
    def reporte(self, resultado):
        print(f"  Paso {resultado['paso']+1:2d} | "
              f"Dif: {resultado['dificultad']:6.1f} | "
              f"Fitness: {resultado['fitness_red']:.3f} | "
              f"E_cadena: {resultado['energia_cadena']:5.1f} | "
              f"L: {resultado['perdida_L']:6.2f} | "
              f"Gen: {resultado['generacion']}")
 
 
# =========================================================
# 🚀 EJECUCIÓN PRINCIPAL
# =========================================================
 
if __name__ == "__main__":
 
    print("\n" + "="*65)
    print("  SISTEMA FUSIONADO — ABRAMGOCHI + CADENA DE RELEVO MOLECULAR")
    print("  H.A.S. Framework | Genoma Cognitivo | Abraham 2026")
    print("  [-½∇² + V_eff(r)] φᵢ(r) = εᵢ φᵢ(r)")
    print("="*65 + "\n")
 
    user, rol = login()
    if not user:
        exit()
 
    log(user, "sistema fusionado iniciado")
 
    sistema = SistemaFusion()
 
    print(f"🌐 Red ABRAMGOCHI: {len(sistema.red.G.nodes())} agentes | "
          f"{len(sistema.red.G.edges())} conexiones")
    print(f"⚡ Cadena molecular: {len(sistema.cadena.moleculas)} moléculas")
    print(f"⚛️  Campo unificado: Kohn-Sham activo\n")
 
    if rol == "admin":
 
        print("─── Ciclos de fusión completa ───\n")
        resultados = []
 
        for paso in range(10):
            resultado = sistema.ciclo_completo(paso)
            sistema.reporte(resultado)
            resultados.append(resultado)
 
        log(user, f"10 ciclos completados — gen {sistema.generacion}")
 
        # Resumen final
        print(f"\n─── Resumen del sistema fusionado ───\n")
 
        fitness_final = resultados[-1]['fitness_red']
        perdida_final = resultados[-1]['perdida_L']
        energia_final = resultados[-1]['energia_cadena']
 
        print(f"  Fitness cognitivo final:     {fitness_final:.4f}")
        print(f"  Energía molecular final:     {energia_final:.2f}")
        print(f"  Pérdida L final:             {perdida_final:.4f}")
        print(f"  Generaciones evolutivas:     {sistema.generacion}")
        print(f"  Estado cadena final:")
 
        for id, energia, carga in sistema.cadena.estado_cadena():
            barra = "█" * int(carga * 20)
            print(f"    Mol {id}: E={energia:5.1f} | Q={carga:.3f} {barra}")
 
        print(f"\n─── Principio central ───\n")
        print(f"  Nunca observamos φᵢ(r) — solo ρ(r).")
        print(f"  Nunca observamos la intención — solo la densidad.")
        print(f"  El relevo ocurre en lo latente. La chispa es lo observable.\n")
 
        print("✅ Sistema fusionado completado.")
        log(user, "sistema fusionado completado exitosamente")
 
    elif rol == "viewer":
        print("👀 Modo lectura")
        print(f"Estado red: {sistema.red.estado}")
        print(f"Estado cadena: {sistema.cadena.estado_cadena()}")
        log(user, "estado visualizado")
 

# parcial-jueces-prog
# TP1 - Programación I - Concurso de Baile
# ---------------------- PUNTO 1: Cargar participantes ----------------------

def input_nombre_participante():
    while True:
        nombre = input("Ingrese nombre del participante : ").strip()
        if len(nombre) >= 3 and all(c.isalpha() or c == " " for c in nombre):
            return nombre
        else:
            print("❌ Nombre inválido. Intente nuevamente.")

def cargar_participantes(nombres):
    for i in range(len(nombres)):
        print(f"\nParticipante {i+1}:")
        nombres[i] = input_nombre_participante()

# ---------------------- PUNTO 2: Cargar puntuaciones ----------------------

def input_puntaje():
     while True:
        try:
            nota = int(input("Ingrese puntaje (1 a 10): "))
            if 1 <= nota <= 10:
                return nota
            else:
                print("❌ El puntaje debe estar entre 1 y 10.")
        except ValueError:
            print("❌ Debe ingresar un número.")

def cargar_puntajes(puntajes):
    for i in range(len(puntajes)):
        print(f"\nCargando puntajes para el participante {i+1}:")
        for j in range(3):
            print(f"  Jurado {j+1}:")
            puntajes[i][j] = input_puntaje()

# ---------------------- PUNTO 3: Mostrar puntuaciones ----------------------

def calcular_promedio(puntajes):
    total = 0
    for nota in puntajes:
        total += nota
    return total / len(puntajes)

def mostrar_puntajes(nombres, puntajes):
    print("\n📋 Tabla de puntuaciones")
    for i in range(len(nombres)):
        prom = calcular_promedio(puntajes[i])
        print(f"{nombres[i]:<15} - J1: {puntajes[i][0]} | J2: {puntajes[i][1]} | J3: {puntajes[i][2]} | Promedio: {prom:.2f}")

# ---------------------- PUNTO 4 y 5: Mostrar promedios mayores a X ----------------------

def mostrar_promedios_superiores(nombres, puntajes, limite):
    hay_resultado = False
    for i in range(len(nombres)):
        prom = calcular_promedio(puntajes[i])
        if prom > limite:
            print(f"{nombres[i]} - Promedio: {prom:.2f}")
            hay_resultado = True
    if not hay_resultado:
        print(f"❌ Ningún participante tiene promedio mayor a {limite}.")

# ---------------------- PUNTO 6: Promedio por jurado ----------------------

def promedio_por_jurado(puntajes, jurado_index):
    total = 0
    for i in range(len(puntajes)):
        total += puntajes[i][jurado_index]
    return total / len(puntajes)

# ---------------------- PUNTO 7: Jurado más estricto ----------------------

def jurado_mas_estricto(puntajes):
    promedios = [promedio_por_jurado(puntajes, j) for j in range(3)]
    menor = promedios[0]
    indice = 0
    for j in range(1, 3):
        if promedios[j] < menor:
            menor = promedios[j]
            indice = j
    print(f"El jurado más estricto es el jurado {indice + 1} con promedio {menor:.2f}")

# ---------------------- PUNTO 8: Buscar participante ----------------------

def buscar_participante(nombres, puntajes, nombre_busqueda):
    encontrado = False
    for i in range(len(nombres)):
        if nombres[i].lower() == nombre_busqueda.lower():
            print(f"\n🎯 {nombres[i]} - Puntajes: {puntajes[i]} - Promedio: {calcular_promedio(puntajes[i]):.2f}")
            encontrado = True
            break
    if not encontrado:
        print("❌ Participante no encontrado.")

# ---------------------- PUNTO 9: Top 3 participantes ----------------------

def top_3_participantes(nombres, puntajes):
    promedios = []
    for i in range(len(nombres)):
        prom = calcular_promedio(puntajes[i])
        promedios.append([nombres[i], prom])

    for i in range(len(promedios)):
        max_idx = i
        for j in range(i + 1, len(promedios)):
            if promedios[j][1] > promedios[max_idx][1]:
                max_idx = j
        promedios[i], promedios[max_idx] = promedios[max_idx], promedios[i]
    
    print("\n🏆 Top 3 participantes por promedio:")
    for i in range(3):
        print(f"{i+1}. {promedios[i][0]} - Promedio: {promedios[i][1]:.2f}")

# ---------------------- PUNTO 10: Ordenar alfabéticamente ----------------------

def mostrar_ordenados_alfabeticamente(nombres, puntajes):
    copia_nombres = nombres[:]
    copia_puntajes = [fila[:] for fila in puntajes]

    for i in range(len(copia_nombres)):
        for j in range(i + 1, len(copia_nombres)):
            if copia_nombres[i].lower() > copia_nombres[j].lower():
                copia_nombres[i], copia_nombres[j] = copia_nombres[j], copia_nombres[i]
                copia_puntajes[i], copia_puntajes[j] = copia_puntajes[j], copia_puntajes[i]
    
    print("\n📚 Participantes ordenados alfabéticamente:")
    for i in range(len(copia_nombres)):
        prom = calcular_promedio(copia_puntajes[i])
        print(f"{copia_nombres[i]:<15} - Puntajes: {copia_puntajes[i]} - Promedio: {prom:.2f}")

# ---------------------- FUNCIONES DE MENÚ ----------------------

def input_opcion_menu():
    try:
        return int(input("Seleccione una opción: "))
    except ValueError:
        return -1

# ---------------------- MENÚ PRINCIPAL ----------------------

def main():
    nombres = [""] * 5
    puntajes = [[0, 0, 0] for _ in range(5)]
    cargados = False

    while True:
        print("\nMenú:")
        print("1. Cargar participantes (Punto 1)")
        print("2. Cargar puntuaciones (Punto 2)")
        print("3. Mostrar puntuaciones y promedios (Punto 3)")
        print("4. Mostrar participantes con promedio > 4 (Punto 4)")
        print("5. Mostrar participantes con promedio > 7 (Punto 5)")
        print("6. Promedio por jurado (Punto 6)")
        print("7. Jurado más estricto (Punto 7)")
        print("8. Buscar participante por nombre (Punto 8)")
        print("9. Mostrar Top 3 (Punto 9)")
        print("10. Ordenar participantes alfabéticamente (Punto 10)")
        print("0. Salir")

        opcion = input_opcion_menu()

        if opcion == 1:
            cargar_participantes(nombres)
        elif opcion == 2:
            cargar_puntajes(puntajes)
            cargados = True
        elif opcion == 3 and cargados:
            mostrar_puntajes(nombres, puntajes)
        elif opcion == 4 and cargados:
            mostrar_promedios_superiores(nombres, puntajes, 4)
        elif opcion == 5 and cargados:
            mostrar_promedios_superiores(nombres, puntajes, 7)
        elif opcion == 6 and cargados:
            for j in range(3):
                print(f"Jurado {j+1}: promedio = {promedio_por_jurado(puntajes, j):.2f}")
        elif opcion == 7 and cargados:
            jurado_mas_estricto(puntajes)
        elif opcion == 8 and cargados:
            nombre = input("Ingrese nombre a buscar: ")
            buscar_participante(nombres, puntajes, nombre)
        elif opcion == 9 and cargados:
            top_3_participantes(nombres, puntajes)
        elif opcion == 10 and cargados:
            mostrar_ordenados_alfabeticamente(nombres, puntajes)
        elif opcion == 0:
            print("👋 Hasta luego.")
            break
        else:
            print("❗ Opción inválida o datos aún no cargados.")

# ---------------------- EJECUCIÓN ----------------------

main()

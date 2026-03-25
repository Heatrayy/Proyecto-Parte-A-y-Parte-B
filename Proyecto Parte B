using System;
namespace SmartPark
{
    class Program
    {
        // ===== MÉTODO: MOSTRAR MENÚ =====
        static void MostrarMenu(string nombreOperador, string codigoTurno, int tiempoSimulado)
        {
            Console.ForegroundColor = ConsoleColor.DarkCyan;
            Console.WriteLine();
            Console.WriteLine("========== MENÚ PRINCIPAL SMARTPARK ==========");
            Console.ResetColor();

            Console.ForegroundColor = ConsoleColor.Gray;
            Console.WriteLine($"Operador: {nombreOperador} | Turno: {codigoTurno} | Tiempo simulado: {tiempoSimulado} min");
            Console.WriteLine("1. Crear ticket de entrada");
            Console.WriteLine("2. Registrar salida y calcular cobro");
            Console.WriteLine("3. Ver estado del parqueo");
            Console.WriteLine("4. Simular paso del tiempo");
            Console.WriteLine("5. Salir");
            Console.ResetColor();
        }

        // ===== MÉTODO: MOSTRAR ESTADO DEL PARQUEO =====
        static void MostrarEstadoParqueo(
            int capacidadParqueo,
            bool ticketActivo,
            int tiempoSimulado,
            decimal dineroRecaudado,
            int ticketsCreados,
            int ticketsCerrados)
        {
            int espaciosOcupados = ticketActivo ? 1 : 0;
            int espaciosDisponibles = capacidadParqueo - espaciosOcupados;

            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("===== ESTADO DEL PARQUEO =====");
            Console.WriteLine($"Capacidad total:       {capacidadParqueo}");
            Console.WriteLine($"Espacios ocupados:     {espaciosOcupados}");
            Console.WriteLine($"Espacios disponibles:  {espaciosDisponibles}");
            Console.WriteLine($"Tiempo simulado:       {tiempoSimulado} minutos");
            Console.WriteLine($"Dinero recaudado:      Q{dineroRecaudado:F2}");
            Console.WriteLine($"Tickets creados:       {ticketsCreados}");
            Console.WriteLine($"Tickets cerrados:      {ticketsCerrados}");
            Console.WriteLine($"Ticket activo:         {(ticketActivo ? "Sí" : "No")}");
            Console.ResetColor();
        }

        // ===== MÉTODO: CREAR TICKET =====
        static void CrearTicket(
            ref bool ticketActivo,
            ref string placaActual,
            ref int tipoVehiculoActual,
            ref string nombreClienteActual,
            ref bool clienteVipActual,
            ref int minutoEntradaActual,
            int tiempoSimulado,
            int capacidadParqueo,
            ref int ticketsCreados)
        {
            if (ticketActivo)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("ERROR: Ya existe un ticket activo. Debe cerrarlo antes de crear uno nuevo.");
                Console.ResetColor();
                return;
            }

            int espaciosOcupados = ticketActivo ? 1 : 0;
            int espaciosDisponibles = capacidadParqueo - espaciosOcupados;

            if (espaciosDisponibles <= 0)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("ERROR: El parqueo está lleno. No se pueden crear más tickets.");
                Console.ResetColor();
                return;
            }

            Console.ForegroundColor = ConsoleColor.Gray;

            // Placa
            string placa;
            do
            {
                Console.Write("Placa del vehículo (6 a 8 caracteres, sin espacios): ");
                placa = Console.ReadLine().Trim();

                if (placa.Length < 6 || placa.Length > 8 || placa.Contains(" "))
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: La placa debe tener entre 6 y 8 caracteres y no contener espacios.");
                    Console.ResetColor();
                    Console.ForegroundColor = ConsoleColor.Gray;
                }
            } while (placa.Length < 6 || placa.Length > 8 || placa.Contains(" "));

            // Tipo de vehículo
            int tipoVehiculo;
            do
            {
                Console.Write("Tipo de vehículo (1 = Moto, 2 = Auto, 3 = Pickup/SUV): ");
                string entradaTipo = Console.ReadLine();

                if (!int.TryParse(entradaTipo, out tipoVehiculo) ||
                    tipoVehiculo < 1 || tipoVehiculo > 3)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: Debe ingresar 1, 2 o 3.");
                    Console.ResetColor();
                    Console.ForegroundColor = ConsoleColor.Gray;
                    tipoVehiculo = 0;
                }
            } while (tipoVehiculo < 1 || tipoVehiculo > 3);

            // Nombre del cliente
            Console.Write("Nombre del cliente: ");
            string nombreCliente = Console.ReadLine();

            // Cliente VIP
            bool esVip = false;
            char respuestaVip;
            do
            {
                Console.Write("¿Cliente VIP? (S/N): ");
                string textoVip = Console.ReadLine().Trim().ToUpper();

                if (textoVip == "S")
                {
                    esVip = true;
                    respuestaVip = 'S';
                }
                else if (textoVip == "N")
                {
                    esVip = false;
                    respuestaVip = 'N';
                }
                else
                {
                    respuestaVip = 'X';
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: Responda únicamente S o N.");
                    Console.ResetColor();
                    Console.ForegroundColor = ConsoleColor.Gray;
                }
            } while (respuestaVip != 'S' && respuestaVip != 'N');

            // Guardar en ticket activo
            placaActual = placa;
            tipoVehiculoActual = tipoVehiculo;
            nombreClienteActual = nombreCliente;
            clienteVipActual = esVip;
            minutoEntradaActual = tiempoSimulado;
            ticketActivo = true;
            ticketsCreados++;

            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Ticket creado correctamente.");
            Console.WriteLine($"Placa: {placaActual} | Cliente: {nombreClienteActual} | Minuto de entrada: {minutoEntradaActual} | VIP: {(clienteVipActual ? "Sí" : "No")}");
            Console.ResetColor();
        }

        // ===== MÉTODO: REGISTRAR SALIDA Y COBRAR =====
        static void RegistrarSalidaYCobro(
            ref bool ticketActivo,
            string placaActual,
            string nombreClienteActual,
            int minutoEntradaActual,
            int tiempoSimulado,
            int tipoVehiculoActual,
            bool clienteVipActual,
            ref decimal dineroRecaudado,
            ref int ticketsCerrados)
        {
            if (!ticketActivo)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("ERROR: No existe un ticket activo para registrar salida.");
                Console.ResetColor();
                return;
            }

            int minutosEstacionado = tiempoSimulado - minutoEntradaActual;
            if (minutosEstacionado < 0)
            {
                minutosEstacionado = 0;
            }

            Console.ForegroundColor = ConsoleColor.Gray;
            Console.WriteLine($"Placa: {placaActual}");
            Console.WriteLine($"Cliente: {nombreClienteActual}");
            Console.WriteLine($"Minuto de entrada: {minutoEntradaActual}");
            Console.WriteLine($"Minuto actual: {tiempoSimulado}");
            Console.WriteLine($"Minutos estacionado: {minutosEstacionado}");

            decimal monto = 0m;

            if (minutosEstacionado <= 15)
            {
                monto = 0m;
            }
            else
            {
                int horasCobradas = minutosEstacionado / 60;
                if (minutosEstacionado % 60 != 0)
                {
                    horasCobradas++;
                }

                decimal tarifaPorHora = 0m;
                if (tipoVehiculoActual == 1)
                {
                    tarifaPorHora = 5m;  // Moto
                }
                else if (tipoVehiculoActual == 2)
                {
                    tarifaPorHora = 10m; // Auto
                }
                else if (tipoVehiculoActual == 3)
                {
                    tarifaPorHora = 15m; // Pickup/SUV
                }

                monto = horasCobradas * tarifaPorHora;

                if (minutosEstacionado > 6 * 60)
                {
                    monto += 25m;
                }

                if (clienteVipActual)
                {
                    decimal descuento = monto * 0.10m;
                    monto -= descuento;
                }
            }

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine($"Monto a pagar: Q{monto:F2}");
            Console.ResetColor();

            dineroRecaudado += monto;
            ticketsCerrados++;
            ticketActivo = false;

            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Ticket cerrado correctamente. ¡Gracias por usar SmartPark!");
            Console.ResetColor();
        }

        // ===== MÉTODO: SIMULAR TIEMPO =====
        static void SimularTiempo(ref int tiempoSimulado)
        {
            int minutosASimular;
            do
            {
                Console.ForegroundColor = ConsoleColor.Gray;
                Console.Write("Ingrese minutos a simular (1 a 1440): ");
                string entradaMinutos = Console.ReadLine();

                if (!int.TryParse(entradaMinutos, out minutosASimular) ||
                    minutosASimular < 1 || minutosASimular > 1440)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: Debe ingresar un entero entre 1 y 1440.");
                    Console.ResetColor();
                    minutosASimular = 0;
                }
            } while (minutosASimular < 1 || minutosASimular > 1440);

            tiempoSimulado += minutosASimular;

            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine($"Tiempo simulado actualizado. Tiempo total: {tiempoSimulado} minutos.");
            Console.ResetColor();
        }

        // ===== MÉTODO PRINCIPAL =====
        static void Main(string[] args)
        {
            Console.Title = "SmartPark - Sistema de Parqueo";

            // ===== REGISTRO INICIAL =====
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("=== INICIO DEL SISTEMA SMARTPARK ===");
            Console.ResetColor();

            Console.ForegroundColor = ConsoleColor.Gray;
            Console.Write("Nombre del operador: ");
            string nombreOperador = Console.ReadLine();

            // Código de turno (exactamente 4 caracteres)
            string codigoTurno;
            do
            {
                Console.Write("Código de turno (exactamente 4 caracteres): ");
                codigoTurno = Console.ReadLine();

                if (codigoTurno.Length != 4)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: El código de turno debe tener exactamente 4 caracteres.");
                    Console.ResetColor();
                }
            } while (codigoTurno.Length != 4);

            // Capacidad del parqueo (mínimo 10)
            int capacidadParqueo;
            do
            {
                Console.Write("Capacidad del parqueo (mínimo 10): ");
                string entradaCapacidad = Console.ReadLine();

                if (!int.TryParse(entradaCapacidad, out capacidadParqueo) || capacidadParqueo < 10)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("ERROR: Ingrese un número entero mayor o igual a 10.");
                    Console.ResetColor();
                }
            } while (capacidadParqueo < 10);

            // Variables del sistema
            int ticketsCreados = 0;
            int ticketsCerrados = 0;
            decimal dineroRecaudado = 0m;
            int tiempoSimulado = 0; // minutos

            // Ticket activo (solo uno a la vez)
            bool ticketActivo = false;
            string placaActual = "";
            int tipoVehiculoActual = 0;
            string nombreClienteActual = "";
            bool clienteVipActual = false;
            int minutoEntradaActual = 0;

            int opcion;

            // ===== CICLO DEL MENÚ =====
            do
            {
                MostrarMenu(nombreOperador, codigoTurno, tiempoSimulado);

                Console.Write("Seleccione una opción (1-5): ");
                string entradaOpcion = Console.ReadLine();
                if (!int.TryParse(entradaOpcion, out opcion))
                {
                    opcion = 0;
                }

                Console.WriteLine();

                switch (opcion)
                {
                    case 1:
                        CrearTicket(
                            ref ticketActivo,
                            ref placaActual,
                            ref tipoVehiculoActual,
                            ref nombreClienteActual,
                            ref clienteVipActual,
                            ref minutoEntradaActual,
                            tiempoSimulado,
                            capacidadParqueo,
                            ref ticketsCreados);
                        break;

                    case 2:
                        RegistrarSalidaYCobro(
                            ref ticketActivo,
                            placaActual,
                            nombreClienteActual,
                            minutoEntradaActual,
                            tiempoSimulado,
                            tipoVehiculoActual,
                            clienteVipActual,
                            ref dineroRecaudado,
                            ref ticketsCerrados);
                        break;

                    case 3:
                        MostrarEstadoParqueo(
                            capacidadParqueo,
                            ticketActivo,
                            tiempoSimulado,
                            dineroRecaudado,
                            ticketsCreados,
                            ticketsCerrados);
                        break;

                    case 4:
                        SimularTiempo(ref tiempoSimulado);
                        break;

                    case 5:
                        Console.ForegroundColor = ConsoleColor.DarkCyan;
                        Console.WriteLine("===== RESUMEN FINAL DEL TURNO =====");
                        Console.ResetColor();

                        Console.ForegroundColor = ConsoleColor.Gray;
                        Console.WriteLine($"Operador:         {nombreOperador}");
                        Console.WriteLine($"Código de turno:  {codigoTurno}");
                        Console.WriteLine($"Tiempo simulado:  {tiempoSimulado} minutos");
                        Console.WriteLine($"Tickets creados:  {ticketsCreados}");
                        Console.WriteLine($"Tickets cerrados: {ticketsCerrados}");
                        Console.WriteLine($"Dinero recaudado: Q{dineroRecaudado:F2}");
                        Console.ResetColor();

                        Console.ForegroundColor = ConsoleColor.Yellow;
                        Console.WriteLine("Fin del programa. Presione una tecla para cerrar...");
                        Console.ResetColor();
                        Console.ReadKey();
                        break;

                    default:
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("ERROR: Opción no válida. Intente nuevamente.");
                        Console.ResetColor();
                        break;
                }

            } while (opcion != 5);
        }
    }
}

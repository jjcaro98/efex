-- Tabla de tipos de vehículo
CREATE TABLE dbo.TipoVehiculo (
    IdTipoVehiculo INT PRIMARY KEY IDENTITY,
    Descripcion NVARCHAR(100) NOT NULL
);

-- Tabla de estados para vehículos
CREATE TABLE dbo.EstadoVehiculo (
    IdEstadoVehiculo INT PRIMARY KEY IDENTITY,
    Descripcion NVARCHAR(50) NOT NULL
);

-- Tabla de estados para reservas
CREATE TABLE dbo.EstadoReserva (
    IdEstadoReserva INT PRIMARY KEY IDENTITY,
    Descripcion NVARCHAR(50) NOT NULL
);

-- Tabla de vehículos
CREATE TABLE dbo.Vehiculo (
    IdVehiculo INT PRIMARY KEY IDENTITY,
    Placa NVARCHAR(20) NOT NULL UNIQUE,
    Marca NVARCHAR(50) NOT NULL,
    Modelo NVARCHAR(50) NOT NULL,
    Anio INT NOT NULL,
    IdTipoVehiculo INT NOT NULL,
    IdEstadoVehiculo INT NOT NULL,
    FOREIGN KEY (IdTipoVehiculo) REFERENCES TipoVehiculo(IdTipoVehiculo),
    FOREIGN KEY (IdEstadoVehiculo) REFERENCES EstadoVehiculo(IdEstadoVehiculo)
);

-- Tabla de clientes
CREATE TABLE dbo.Cliente (
    IdCliente INT PRIMARY KEY IDENTITY,
    Nombre NVARCHAR(100) NOT NULL,
    Apellido NVARCHAR(100) NOT NULL,
    Documento NVARCHAR(20) NOT NULL UNIQUE,
    Email NVARCHAR(100),
    Telefono NVARCHAR(20)
);

-- Tabla de reservas
CREATE TABLE dbo.Reserva (
    IdReserva INT PRIMARY KEY IDENTITY,
    IdVehiculo INT NOT NULL,
    IdCliente INT NOT NULL,
    FechaInicio DATE NOT NULL,
    FechaFin DATE NOT NULL,
    IdEstadoReserva INT NOT NULL,
    FOREIGN KEY (IdVehiculo) REFERENCES Vehiculo(IdVehiculo),
    FOREIGN KEY (IdCliente) REFERENCES Cliente(IdCliente),
    FOREIGN KEY (IdEstadoReserva) REFERENCES EstadoReserva(IdEstadoReserva)
);

-- Tabla de reportes diarios
CREATE TABLE dbo.ReporteDiario (
    IdReporte INT PRIMARY KEY IDENTITY,
    Fecha DATE NOT NULL,
    TotalReservas INT NOT NULL,
    Detalle NVARCHAR(MAX)
);

-- Inserts para TipoVehiculo
INSERT INTO dbo.TipoVehiculo (Descripcion) VALUES ('Sedán');
INSERT INTO dbo.TipoVehiculo (Descripcion) VALUES ('SUV');
INSERT INTO dbo.TipoVehiculo (Descripcion) VALUES ('Pickup');
INSERT INTO dbo.TipoVehiculo (Descripcion) VALUES ('Hatchback');
INSERT INTO dbo.TipoVehiculo (Descripcion) VALUES ('Van');

-- Inserts para EstadoVehiculo
INSERT INTO dbo.EstadoVehiculo (Descripcion) VALUES ('Disponible');
INSERT INTO dbo.EstadoVehiculo (Descripcion) VALUES ('Alquilado');
INSERT INTO dbo.EstadoVehiculo (Descripcion) VALUES ('Mantenimiento');

-- Inserts para EstadoReserva
INSERT INTO dbo.EstadoReserva (Descripcion) VALUES ('Activa');
INSERT INTO dbo.EstadoReserva (Descripcion) VALUES ('Finalizada');
INSERT INTO dbo.EstadoReserva (Descripcion) VALUES ('Cancelada');
SELECT * FROM dbo.Vehiculo
-- Ejemplo de inserts para Vehiculo (usando los IDs de los inserts anteriores)
INSERT INTO dbo.Vehiculo (Placa, Marca, Modelo, Anio, IdTipoVehiculo, IdEstadoVehiculo) VALUES ('ABC123', 'Toyota', 'Corolla', 2022, 1, 1);
INSERT INTO dbo.Vehiculo (Placa, Marca, Modelo, Anio, IdTipoVehiculo, IdEstadoVehiculo) VALUES ('DEF456', 'Mazda', 'CX-5', 2023, 2, 1);
INSERT INTO dbo.Vehiculo (Placa, Marca, Modelo, Anio, IdTipoVehiculo, IdEstadoVehiculo) VALUES ('GHI789', 'Ford', 'Ranger', 2021, 3, 3);
INSERT INTO dbo.Vehiculo (Placa, Marca, Modelo, Anio, IdTipoVehiculo, IdEstadoVehiculo) VALUES ('JKL012', 'Kia', 'Rio', 2020, 4, 1);
INSERT INTO dbo.Vehiculo (Placa, Marca, Modelo, Anio, IdTipoVehiculo, IdEstadoVehiculo) VALUES ('MNO345', 'Hyundai', 'H1', 2022, 5, 2);

INSERT INTO dbo.Cliente (Nombre, Apellido, Documento, Email, Telefono)
VALUES ('Juan', 'Pérez', '12345678', 'juan.perez@email.com', '555-1234');

INSERT INTO dbo.Reserva (IdVehiculo, IdCliente, FechaInicio, FechaFin, IdEstadoReserva)
VALUES (
    (SELECT IdVehiculo FROM dbo.Vehiculo WHERE Placa = 'ABC123'), -- IdVehiculo
    1,                                                           -- IdCliente
    '2025-05-20',                                                -- FechaInicio
    '2025-05-25',                                                -- FechaFin
    1                                                            -- IdEstadoReserva (Activa)
);

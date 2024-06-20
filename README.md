# ED
===============================================================================================================================
Шаг 1: Определение сущностей и атрибутов
На основе описания предметной области из задания, идентифицируйте основные сущности и их атрибуты:

Пациенты

PatientID (PK)
Имя
Фамилия
Дата рождения
Пол
Контактный номер телефона
Адрес электронной почты
Место жительства
Врачи

DoctorID (PK)
Имя
Фамилия
Специализация
Офис
Контактный номер телефона
Адрес электронной почты
Записи на прием

AppointmentID (PK)
PatientID (FK)
DoctorID (FK)
Дата приема
Время приема
Описание проблемы
Медицинские записи

RecordID (PK)
PatientID (FK)
DoctorID (FK)
Дата записи
Описание
Администраторы

AdminID (PK)
Имя
Фамилия
Роль
Шаг 2: Определение связей и ключей
Определите связи между сущностями и установите внешние ключи:

Пациенты и Врачи

Пациенты могут иметь множество записей на прием к врачам.
Один врач может принимать множество пациентов.
Записи на прием

Запись на прием связана с одним пациентом и одним врачом.
Медицинские записи

Медицинская запись относится к одному пациенту и одному врачу.
Администраторы

Администраторы управляют базой данных врачей и пациентов.
Шаг 3: Проектирование ER-диаграммы
Используйте программное обеспечение для создания ER-диаграммы (например, MySQL Workbench, Microsoft Visio или онлайн-инструменты типа draw.io).

Шаг 4: Обеспечение нормализации до 3NF
Убедитесь, что ваша ER-диаграмма находится в третьей нормальной форме:

Пациенты

Атрибуты напрямую зависят от первичного ключа PatientID.
Врачи

Атрибуты напрямую зависят от первичного ключа DoctorID.
Записи на прием

Атрибуты напрямую зависят от первичного ключа AppointmentID, а PatientID и DoctorID выступают внешними ключами.
Медицинские записи

Атрибуты напрямую зависят от первичного ключа RecordID, а PatientID и DoctorID выступают внешними ключами.
Администраторы

Атрибуты напрямую зависят от первичного ключа AdminID.
Шаг 5: Обеспечение ссылочной целостности
Убедитесь, что все внешние ключи правильно установлены и обеспечивают ссылочную целостность:

В таблице Записи на прием внешние ключи PatientID и DoctorID ссылаются на соответствующие первичные ключи в таблицах Пациенты и Врачи.
В таблице Медицинские записи внешние ключи PatientID и DoctorID ссылаются на соответствующие первичные ключи в таблицах Пациенты и Врачи.
Шаг 6: Создание базы данных и заполнение тестовыми данными
Создайте базу данных, используя предпочтительную платформу (например, MySQL, PostgreSQL):

sql
Копировать код
-- Создание таблиц
CREATE TABLE Patients (
    PatientID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    BirthDate DATE,
    Gender VARCHAR(10),
    PhoneNumber VARCHAR(15),
    Email VARCHAR(50),
    Address VARCHAR(100)
);

CREATE TABLE Doctors (
    DoctorID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Specialization VARCHAR(50),
    Office VARCHAR(20),
    PhoneNumber VARCHAR(15),
    Email VARCHAR(50)
);

CREATE TABLE Appointments (
    AppointmentID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    AppointmentDate DATE,
    AppointmentTime TIME,
    ProblemDescription TEXT,
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID)
);

CREATE TABLE MedicalRecords (
    RecordID INT PRIMARY KEY,
    PatientID INT,
    DoctorID INT,
    RecordDate DATE,
    Description TEXT,
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID)
);

CREATE TABLE Administrators (
    AdminID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Role VARCHAR(50)
);

-- Вставка тестовых данных
INSERT INTO Patients (PatientID, FirstName, LastName, BirthDate, Gender, PhoneNumber, Email, Address) VALUES
(1, 'Alice', 'Johnson', '1990-01-01', 'Female', '1234567890', 'alice@example.com', '123 Main St'),
(2, 'Bob', 'Smith', '1985-05-12', 'Male', '0987654321', 'bob@example.com', '456 Elm St');

INSERT INTO Doctors (DoctorID, FirstName, LastName, Specialization, Office, PhoneNumber, Email) VALUES
(1, 'Dr. John', 'Doe', 'Cardiology', 'Room 101', '1112223333', 'john.doe@example.com'),
(2, 'Dr. Jane', 'Smith', 'Dermatology', 'Room 102', '4445556666', 'jane.smith@example.com');

INSERT INTO Appointments (AppointmentID, PatientID, DoctorID, AppointmentDate, AppointmentTime, ProblemDescription) VALUES
(1, 1, 1, '2024-07-01', '10:00:00', 'Regular Checkup'),
(2, 2, 2, '2024-07-02', '11:00:00', 'Skin Rash');

INSERT INTO MedicalRecords (RecordID, PatientID, DoctorID, RecordDate, Description) VALUES
(1, 1, 1, '2024-06-20', 'Blood Test Results: Normal'),
(2, 2, 2, '2024-06-21', 'Skin Test Results: Allergy to pollen');

INSERT INTO Administrators (AdminID, FirstName, LastName, Role) VALUES
(1, 'Admin', 'User', 'System Administrator');
=======================================================================================================================================================================================
Для выполнения задания по разработке API в модуле 3, следуйте следующим шагам. Пример кода будет на C# с использованием ASP.NET Core.

Шаги для создания API
Создание нового пациента:

Endpoint: POST /patients
Параметры:
name (string)
surname (string)
date_of_birth (string, формат: YYYY-MM-DD)
gender (string)
phone_number (string)
email (string)
address (string)
Ответ: Возвращает ID нового пациента.
Получение всех пациентов:

Endpoint: GET /patients
Ответ: Возвращает список всех пациентов с их данными.
Получение информации о конкретном пациенте:

Endpoint: GET /patients/{id}
Параметры: id (integer, ID пациента)
Ответ: Возвращает данные указанного пациента.
Обновление информации о пациенте:

Endpoint: PUT /patients/{id}
Параметры:
id (integer, ID пациента)
name (string)
surname (string)
date_of_birth (string, формат: YYYY-MM-DD)
gender (string)
phone_number (string)
email (string)
address (string)
Ответ: Возвращает обновленные данные пациента.
Удаление пациента:

Endpoint: DELETE /patients/{id}
Параметры: id (integer, ID пациента)
Ответ: Возвращает сообщение об успешном удалении.
Создание нового приема:

Endpoint: POST /appointments
Параметры:
patient_id (integer, ID пациента)
doctor_id (integer, ID врача)
appointment_date (string, формат: YYYY-MM-DD HH
)
reason (string, причина приема)
Ответ: Возвращает ID нового приема.
Получение всех приемов:

Endpoint: GET /appointments
Ответ: Возвращает список всех приемов с их данными.
Получение информации о конкретном приеме:

Endpoint: GET /appointments/{id}
Параметры: id (integer, ID приема)
Ответ: Возвращает данные указанного приема.
Обновление информации о приеме:

Endpoint: PUT /appointments/{id}
Параметры:
id (integer, ID приема)
patient_id (integer, ID пациента)
doctor_id (integer, ID врача)
appointment_date (string, формат: YYYY-MM-DD HH
)
reason (string, причина приема)
Ответ: Возвращает обновленные данные приема.
Удаление приема:

Endpoint: DELETE /appointments/{id}
Параметры: id (integer, ID приема)
Ответ: Возвращает сообщение об успешном удалении.
Пример кода на C# с использованием ASP.NET Core
Создайте новый проект ASP.NET Core Web API и добавьте необходимые контроллеры и модели.

Модель пациента:

csharp
Копировать код
public class Patient
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Surname { get; set; }
    public DateTime DateOfBirth { get; set; }
    public string Gender { get; set; }
    public string PhoneNumber { get; set; }
    public string Email { get; set; }
    public string Address { get; set; }
}
Модель приема:

csharp
Копировать код
public class Appointment
{
    public int Id { get; set; }
    public int PatientId { get; set; }
    public int DoctorId { get; set; }
    public DateTime AppointmentDate { get; set; }
    public string Reason { get; set; }
}
Контроллер для пациентов:

csharp
Копировать код
[ApiController]
[Route("api/[controller]")]
public class PatientsController : ControllerBase
{
    private static List<Patient> patients = new List<Patient>();
    private static int nextId = 1;

    [HttpPost]
    public IActionResult CreatePatient([FromBody] Patient patient)
    {
        patient.Id = nextId++;
        patients.Add(patient);
        return CreatedAtAction(nameof(GetPatient), new { id = patient.Id }, patient);
    }

    [HttpGet]
    public IActionResult GetPatients()
    {
        return Ok(patients);
    }

    [HttpGet("{id}")]
    public IActionResult GetPatient(int id)
    {
        var patient = patients.FirstOrDefault(p => p.Id == id);
        if (patient == null)
        {
            return NotFound();
        }
        return Ok(patient);
    }

    [HttpPut("{id}")]
    public IActionResult UpdatePatient(int id, [FromBody] Patient updatedPatient)
    {
        var patient = patients.FirstOrDefault(p => p.Id == id);
        if (patient == null)
        {
            return NotFound();
        }
        patient.Name = updatedPatient.Name;
        patient.Surname = updatedPatient.Surname;
        patient.DateOfBirth = updatedPatient.DateOfBirth;
        patient.Gender = updatedPatient.Gender;
        patient.PhoneNumber = updatedPatient.PhoneNumber;
        patient.Email = updatedPatient.Email;
        patient.Address = updatedPatient.Address;
        return Ok(patient);
    }

    [HttpDelete("{id}")]
    public IActionResult DeletePatient(int id)
    {
        var patient = patients.FirstOrDefault(p => p.Id == id);
        if (patient == null)
        {
            return NotFound();
        }
        patients.Remove(patient);
        return Ok(new { message = "Patient deleted" });
    }
}
Контроллер для приемов:

csharp
Копировать код
[ApiController]
[Route("api/[controller]")]
public class AppointmentsController : ControllerBase
{
    private static List<Appointment> appointments = new List<Appointment>();
    private static int nextId = 1;

    [HttpPost]
    public IActionResult CreateAppointment([FromBody] Appointment appointment)
    {
        appointment.Id = nextId++;
        appointments.Add(appointment);
        return CreatedAtAction(nameof(GetAppointment), new { id = appointment.Id }, appointment);
    }

    [HttpGet]
    public IActionResult GetAppointments()
    {
        return Ok(appointments);
    }

    [HttpGet("{id}")]
    public IActionResult GetAppointment(int id)
    {
        var appointment = appointments.FirstOrDefault(a => a.Id == id);
        if (appointment == null)
        {
            return NotFound();
        }
        return Ok(appointment);
    }

    [HttpPut("{id}")]
    public IActionResult UpdateAppointment(int id, [FromBody] Appointment updatedAppointment)
    {
        var appointment = appointments.FirstOrDefault(a => a.Id == id);
        if (appointment == null)
        {
            return NotFound();
        }
        appointment.PatientId = updatedAppointment.PatientId;
        appointment.DoctorId = updatedAppointment.DoctorId;
        appointment.AppointmentDate = updatedAppointment.AppointmentDate;
        appointment.Reason = updatedAppointment.Reason;
        return Ok(appointment);
    }

    [HttpDelete("{id}")]
    public IActionResult DeleteAppointment(int id)
    {
        var appointment = appointments.FirstOrDefault(a => a.Id == id);
        if (appointment == null)
        {
            return NotFound();
        }
        appointments.Remove(appointment);
        return Ok(new { message = "Appointment deleted" });
    }
}
======================================================================================================================================
Для выполнения задания "разработка тестовых наборов" из файла, вам нужно создать тестовые данные для проверки даты записи клиента и номера талона. Ниже приведены шаги и пример реализации на C# с использованием фреймворка тестирования xUnit.

Шаги выполнения задания "разработка тестовых наборов"
Создайте проект тестирования с использованием xUnit в Visual Studio:

Откройте Visual Studio.
Создайте новый проект, выбрав "xUnit Test Project".
Добавьте тестовые данные для проверки даты записи клиента и номера талона:

В тестовом проекте создайте тесты, которые будут проверять корректность ввода даты и номера талона.
Пример кода на C#
1. Создайте модель для записи на прием (Appointment)
csharp
Копировать код
public class Appointment
{
    public DateTime Date { get; set; }
    public int TicketNumber { get; set; }

    public bool IsValidDate()
    {
        return Date > DateTime.Now;
    }

    public bool IsValidTicketNumber()
    {
        return TicketNumber > 0;
    }
}
2. Напишите тесты для проверки корректности даты и номера талона
csharp
Копировать код
using System;
using Xunit;

public class AppointmentTests
{
    [Fact]
    public void AppointmentDate_ShouldBeInTheFuture()
    {
        // Arrange
        var appointment = new Appointment { Date = DateTime.Now.AddDays(1) };

        // Act
        var isValid = appointment.IsValidDate();

        // Assert
        Assert.True(isValid, "Appointment date should be in the future.");
    }

    [Fact]
    public void AppointmentDate_ShouldNotBeInThePast()
    {
        // Arrange
        var appointment = new Appointment { Date = DateTime.Now.AddDays(-1) };

        // Act
        var isValid = appointment.IsValidDate();

        // Assert
        Assert.False(isValid, "Appointment date should not be in the past.");
    }

    [Theory]
    [InlineData(1)]
    [InlineData(100)]
    public void TicketNumber_ShouldBePositive(int ticketNumber)
    {
        // Arrange
        var appointment = new Appointment { TicketNumber = ticketNumber };

        // Act
        var isValid = appointment.IsValidTicketNumber();

        // Assert
        Assert.True(isValid, "Ticket number should be positive.");
    }

    [Theory]
    [InlineData(0)]
    [InlineData(-5)]
    public void TicketNumber_ShouldNotBeNonPositive(int ticketNumber)
    {
        // Arrange
        var appointment = new Appointment { TicketNumber = ticketNumber };

        // Act
        var isValid = appointment.IsValidTicketNumber();

        // Assert
        Assert.False(isValid, "Ticket number should be positive.");
    }
}
Объяснение кода
Модель Appointment:

Date: Дата записи на прием.
TicketNumber: Номер талона.
IsValidDate(): Метод проверяет, что дата записи на прием находится в будущем.
IsValidTicketNumber(): Метод проверяет, что номер талона является положительным числом.
Тесты:

AppointmentDate_ShouldBeInTheFuture: Проверяет, что дата записи на прием находится в будущем.
AppointmentDate_ShouldNotBeInThePast: Проверяет, что дата записи на прием не находится в прошлом.
TicketNumber_ShouldBePositive: Проверяет, что номер талона является положительным числом.
TicketNumber_ShouldNotBeNonPositive: Проверяет, что номер талона не является неположительным числом (0 или отрицательное значение).
Запуск тестов
Откройте Test Explorer в Visual Studio.
Запустите тесты и убедитесь, что они проходят успешно.
Таким образом, вы создадите тестовые наборы для проверки корректности даты записи клиента и номера талона на C# с использованием xUnit.

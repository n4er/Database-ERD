Added the diagram for the company's ERD system as well as the relational diagram between them and moved the primary and foreign keys

<pre><code class="language-sql">
-- 1. Department Table (Strong Entity)
Department (
    DNUM INT PRIMARY KEY,
    DName VARCHAR(50) NOT NULL
);

-- 2. Employee Table (Strong Entity with Recursive FK)
Employee (
    SSN INT PRIMARY KEY,
    Fname VARCHAR(50) NOT NULL,
    Lname VARCHAR(50) NOT NULL,
    BirthDate DATE NOT NULL,
    Gender VARCHAR(10),
    DNUM INT NOT NULL,
    SuperSSN INT,  -- Nullable for top-level managers
    FOREIGN KEY (DNUM) REFERENCES Department(DNUM),
    FOREIGN KEY (SuperSSN) REFERENCES Employee(SSN)
);

-- 3. Dependent Table (Weak Entity, Composite PK)
Dependent (
    SSN INT,  -- FK to Employee
    DependentName VARCHAR(50),
    Gender VARCHAR(10),
    BirthDate DATE,
    PRIMARY KEY (SSN, DependentName),
    FOREIGN KEY (SSN) REFERENCES Employee(SSN)
);

-- 4. DepartmentLocations (Multivalued Attribute)
DepartmentLocations (
    DNUM INT,
    Location VARCHAR(50),
    PRIMARY KEY (DNUM, Location),
    FOREIGN KEY (DNUM) REFERENCES Department(DNUM)
);

-- 5. Project Table (Strong Entity)
Project (
    PNumber INT PRIMARY KEY,
    PName VARCHAR(50) NOT NULL,
    Location VARCHAR(50),
    City VARCHAR(50),
    DNUM INT NOT NULL,
    FOREIGN KEY (DNUM) REFERENCES Department(DNUM)
);

-- 6. WorksOn Table (M:N Relationship with Attribute)
WorksOn (
    SSN INT,
    PNumber INT,
    WorkingHours FLOAT,
    PRIMARY KEY (SSN, PNumber),
    FOREIGN KEY (SSN) REFERENCES Employee(SSN),
    FOREIGN KEY (PNumber) REFERENCES Project(PNumber)
);

-- 7. DepartmentManager Table (1:1 or 1:N with Attribute)
-- Retained for historical management tracking
DepartmentManager (
    DNUM INT,
    SSN INT,
    HireDate DATE,
    PRIMARY KEY (DNUM, SSN),
    FOREIGN KEY (DNUM) REFERENCES Department(DNUM),
    FOREIGN KEY (SSN) REFERENCES Employee(SSN)
);
</code></pre>

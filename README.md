# entrega plpgsql solucion

## 1

      CREATE OR REPLACE FUNCTION get_employee_count() 
      RETURNS void AS $$ 
      DECLARE 
          codigo departments.department_id%TYPE; 
          c1 CURSOR (cod departments.department_id%TYPE) FOR 
              SELECT COUNT(*) FROM employees WHERE department_id=cod; 
          num_emple INT; 
      BEGIN 
          codigo:=10; 
          OPEN c1(codigo); 
          FETCH c1 INTO num_emple; 
          RAISE NOTICE 'numero de empleados de % es %', codigo, num_emple; 
      END; $$ 
      LANGUAGE plpgsql;

## 2
      BEGIN
          FOR EMPLE IN SELECT * FROM EMPLOYEES WHERE JOB_ID = 'ST_CLERK' LOOP
              RAISE NOTICE '%', EMPLE.FIRST_NAME;
          END LOOP;
      END;

## 3 como bloque --> https://www.postgresql.org/docs/current/sql-do.html

      DO $$
      DECLARE 
      CURSOR C1 IS SELECT * FROM Employees FOR UPDATE; 
      EMPLEADO RECORD;
      BEGIN 
      FOR EMPLEADO IN C1 LOOP 
      IF EMPLEADO.SALARY > 8000 THEN 
      UPDATE EMPLOYEES SET SALARY=SALARY*1.02 
      WHERE CURRENT OF C1; 
      ELSE 
      UPDATE EMPLOYEES SET SALARY=SALARY*1.03 
      WHERE CURRENT OF C1; 
      END IF; 
      END LOOP; 
      COMMIT; 
      END $$;

## 3 como procedimiento

      CREATE OR REPLACE PROCEDURE ACTUALIZA_SALARIO_EMPLEADOS() 
      LANGUAGE plpgsql
      AS $$
      DECLARE 
          CURSOR C1 IS SELECT * FROM Employees FOR UPDATE; 
          EMPLEADO Employees%ROWTYPE;
      BEGIN 
          FOR EMPLEADO IN C1 LOOP 
              IF EMPLEADO.SALARY > 8000 THEN 
                  UPDATE Employees SET SALARY=SALARY*1.02 WHERE CURRENT OF C1; 
              ELSE 
                  UPDATE Employees SET SALARY=SALARY*1.03 WHERE CURRENT OF C1; 
              END IF; 
          END LOOP; 
          COMMIT; 
      END;
      $$;


# entrega plpgsql solucion

## 1

      CREATE OR REPLACE FUNCTION get_employee_count() 
      RETURNS void AS $$ 
      DECLARE 
          codigo departments.department_id%TYPE; 
          CURSOR c1(cod departments.department_id%TYPE) IS 
              SELECT COUNT(*) FROM employees WHERE department_id=cod; 
          num_emple number; 
      BEGIN 
          codigo:=10; 
          OPEN c1(codigo); 
          FETCH c1 INTO num_emple; 
          RAISE NOTICE 'numero de empleados de % es %', codigo, num_emple; 
      END; $$ 
      LANGUAGE plpgsql;

## 2

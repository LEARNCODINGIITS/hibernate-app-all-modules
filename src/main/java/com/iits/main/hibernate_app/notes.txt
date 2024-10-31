package com.iits.main.hibernate_app;

import java.io.IOException;
import java.util.List;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

import jakarta.persistence.criteria.CriteriaBuilder;
import jakarta.persistence.criteria.CriteriaQuery;
import jakarta.persistence.criteria.Predicate;
import jakarta.persistence.criteria.Root;

public class App  {
    @SuppressWarnings("deprecation")
	public static void main( String[] args ) throws IOException {
         //Step1:Configuration
    	Configuration cfg= new Configuration();
    	   cfg.configure("hibernate.cfg.xml");
    	//Step2: Get the SessionFactory object
    	SessionFactory sf=cfg.buildSessionFactory();
    	Session session=sf.openSession();
    	/*Transaction tx=	session.beginTransaction();
     
    	Employee employee =new Employee();
    	employee.setEid(4);
    	employee.setEname("SR");
    	employee.setEsalary(60000.00);
    	session.persist(employee);
    	System.out.println(session.contains(employee));
    	System.in.read();
    	System.in.read();
    	//Object o=session.save("save", employee);
    	//System.out.println(o);
    	tx.commit();*/
    	//Employee e=session.load(Employee.class, 1);
    	//System.out.println(e);
    	//Employee e=session.get(Employee.class, 1);
    	//System.out.println(e);
    	
    	//Query<?> q=session.createQuery("from Employee e");
    	//q.list().forEach(System.out::print);
    	
    	//Query<?> q=session.createQuery("from Employee e where e.eid=:eid");
    	//q.setParameter("eid", 1);
    //	Query<?> q=session.createQuery("from Employee e where e.eid=?1");
    	//q.setParameter(1,1);
    	
    	//Object o=q.getSingleResultOrNull();
    	//System.out.println(o);
    	
    	//org.hibernate.Transaction tx=session.beginTransaction();
    	
    	/*Query<?> q=
    			session.createQuery("update Employee e SET e.esalary=:esalary WHERE e.eid=:eid");
    	
    	q.setParameter("esalary", 60000.00);
    	q.setParameter("eid", 1);
    	int i=q.executeUpdate();
    	System.out.println(i);tx.commit*/
/*
    	org.hibernate.Transaction tx=session.beginTransaction();

    	Query<?> q=
    			session.createQuery("delete from Employee e WHERE e.eid=:eid");
    	
    	q.setParameter("eid", 3);
    	int i=q.executeUpdate();
    	System.out.println(i+" record is deleted");
    	tx.commit();*/
  
    	//NativeSQL Queries
    	/*Query q=session.createNativeQuery("select * From Employee",Employee.class);
    	System.out.println(q.list());*/
    	
    	//Criteria API
    	/*CriteriaQuery<Employee> cq=session.getCriteriaBuilder().createQuery(Employee.class);
        Root<Employee> root=cq.from(Employee.class);
    	cq.select(root);
    	List<Employee> employee=session.createQuery(cq).getResultList();
    	employee.forEach(System.out::println);
    	*/
    	//Create Builder
    	CriteriaBuilder cBuilder=session.getCriteriaBuilder();
    	//Create CriteriaQuery for Employee
    	CriteriaQuery<Employee> cq=cBuilder.createQuery(Employee.class);
    	//Define the root of the query
    	Root<Employee> root=cq.from(Employee.class);
    	//create a predicate for salary greater then the threshold
        Predicate predicate=cBuilder.gt(root.get("esalary"),30000.00);
        cq.select(root).where(predicate);
    	
        List<Employee> list=session.createQuery(cq).getResultList();
        list.forEach(System.out::println);
    	session.close();
    	sf.close();
    }
}

package logic;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

import javax.swing.JOptionPane;

import data.Project;

public class DatabaseManager {
	private Connection con = null;
	private Statement cmd = null;
	private ResultSet reader = null;

	private ArrayList<Project> projList;

	
	public ArrayList<Project> getData(String qry) throws Exception {
		Class.forName("com.mysql.jdbc.Driver");
		con = setConnection(); // Call connection string method below
		cmd = con.createStatement(); // Ready/Create a statement(SQL)
		try {
			reader = cmd.executeQuery(qry);
			projList = new ArrayList<Project>();
			while (reader.next()) {
				Project proj = new Project();
				proj.setProjectID(reader.getString("id"));
				proj.setProjectCommonName(reader.getString("projectcommonname"));
				proj.setProjectName(reader.getString("nspprojectname"));
				proj.setManager(reader.getString("name"));
				projList.add(proj);
			}
		} catch (SQLException e) {
			JOptionPane.showMessageDialog(null, "Error in retrieving from database!", "ERROR",
					JOptionPane.ERROR_MESSAGE);// Display the error occured
		} finally {
			con.close(); // Close the connection string
		}
		return projList;
	}

	private Connection setConnection() throws Exception {
		return DriverManager.getConnection("jdbc:mysql://localhost:3306/bspgoss_prod_oss-1.0.5", "root", "root"); // CONFIG
	}

	public Project getProjectDetails(String qry) throws Exception {
		Project proj = null;
		Class.forName("com.mysql.jdbc.Driver");
		con = setConnection(); // Call connection string method below
		cmd = con.createStatement(); // Ready/Create a statement(SQL)
		try {
			reader = cmd.executeQuery(qry);
			while (reader.next()) {
				proj = new Project();
				proj.setProjectID(reader.getString("id"));
				proj.setProjectCommonName(reader.getString("projectcommonname"));
				proj.setProjectName(reader.getString("nspprojectname"));
				proj.setManager(reader.getString("name"));
			}
		} catch (SQLException e) {
			JOptionPane.showMessageDialog(null, "Error in retrieving from database!", "ERROR",
					JOptionPane.ERROR_MESSAGE);// Display the error occured
		} finally {
			con.close(); // Close the connection string
		}
		return proj;

	}
}

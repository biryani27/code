# code
12.Consider the Database db_LSA (Lecturer Subject Allocation) consisting of the following tables: 
tbl_Subjects(IdSubject: int,  SubjectName: string) 
tbl_Lecturers(IdLecturer: int, LecturerName: string) 
tbl_LecturerSubjects(LecturerName: string,  SubjectName: string) 
Develop a suitable window application using C#.NET having following options. 
1. Enter new Subject Details. 
2. Enter New Lecturer Details. 
3. Subject Allocation with Lecturer Name in a Combo box and subjects to be allocated in Grid with checkbox Column. 
4. Display all the subjects allocated (In a Grid) to the selected Lecturer (In a Combo Box).
Form Design
 
OUTPUT
 

 

 

 
C# Code

using System.Data;
using System.Data.SqlClient;

namespace WinFormsApp3
{
    public partial class Form1 : Form
    {
        SqlConnection con = new SqlConnection(@"Data Source=NITHIN-LAP\SQLEXPRESS;Initial Catalog = master; Integrated Security = True");
        SqlCommand cmd = new SqlCommand();
        SqlDataReader dr;
        SqlDataAdapter da;
        DataSet ds = new DataSet();
        DataTable dt = new DataTable();

        public Form1()
        {
            InitializeComponent();
        }


        private void Form1_Load(object sender, EventArgs e)
        {
            dt.Reset();
            con.Open();
            ds.Reset();
            ds.Tables.Add(dt);
            da = new SqlDataAdapter("select SubjectName from tbl_Subjects", con);
            da.Fill(dt);
            dataGridView1.DataSource = dt.DefaultView;
            con.Close();
            cmd.Connection = con;
            DataGridViewCheckBoxColumn cb = new DataGridViewCheckBoxColumn();
            cb.Name = "cbc";
            dataGridView1.Columns.Insert(0, cb);
            loadlist();
        }
        public void loadlist()
        {
            con.Open();
            cmd.CommandText = "select * from tbl_Lecturers";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    comboBox1.Items.Add(dr[1].ToString());
                    comboBox2.Items.Add(dr[1].ToString());
                }
            }
            con.Close();
        }

        

        private void button2_Click(object sender, EventArgs e)
        {
            con.Open();
            cmd.CommandText = "insert into tbl_Lecturers values('" + textBox3.Text
           + "','" + textBox4.Text + "')";
            cmd.ExecuteNonQuery();
            cmd.Clone();
            MessageBox.Show("Record inserted");
            con.Close();
            textBox3.Text = "";
            textBox4.Text = "";

        }


        private void button1_Click(object sender, EventArgs e)
        {
            cmd.Connection = con;
            con.Open();
            cmd.CommandText = "insert into tbl_Subjects values('" + textBox1.Text
           + "','" + textBox2.Text + "')";
            cmd.ExecuteNonQuery();
            cmd.Clone();
            MessageBox.Show("Record inserted");
            con.Close();
            textBox1.Text = "";
            textBox2.Text = "";

        }

        private void comboBox2_SelectedIndexChanged(object sender, EventArgs e)
        {
            DataSet ds1 = new DataSet();
            DataTable dt1 = new DataTable();
            con.Open();
            ds1.Reset();
            ds1.Tables.Add(dt1);
            SqlDataAdapter da1 = new SqlDataAdapter("select * from tbl_LecturerSubjects where LecturerName = '" + comboBox2.SelectedItem.ToString() + "'", con);


            da1.Fill(dt1);
            dataGridView2.DataSource = dt1.DefaultView;
            con.Close();
        }

        private void button3_Click(object sender, EventArgs e)
        {
            int i = 0;

            foreach (DataGridViewRow row in dataGridView1.Rows)
            {
                bool iss = Convert.ToBoolean(row.Cells["cbc"].Value);
                if (iss)
                {
                    cmd.CommandText = "insert into tbl_LecturerSubjects values('" + comboBox1.SelectedItem.ToString() + "', '" + row.Cells[1].Value + "')";


                    con.Open();
                    cmd.ExecuteNonQuery();
                    con.Close();
                    MessageBox.Show("inserted successfully");
                }
            }
            i++;
        }

    }
}





13) Consider the database db_VSS (Vehicle Service Station) consisting of the following tables: 

tbl_VehicleTypes(IdVehicleType: int, VehicleType: string, ServiceCharge: int) tbl_ServiceDetails(IdService: int, VehicleNumber: string, ServiceDetails: string, IdVehicleType: int) 

Develop a suitable window application using C#.NET having following options. 
1. Enter new Service Details for the Selected Vehicle Type (In a Combo Box). 
2. Update the Existing Service Charges to Database.
3. Total Service Charges Collected for the Selected Vehicle (In a Combo box) with total amount displayed in a text box. NOTE: tbl_VehicleType is a static table containing the following Rows in it. 1 Two Wheeler 500 
2 Four Wheeler 1000 
3 Three Wheeler 700

Design:

 

 
Code:
using System.Data;
using System.Data.SqlClient;

namespace VehicleServiceStation
{
    public partial class Form1 : Form
    {
        SqlConnection con = new SqlConnection(@"Data Source=NITHIN-LAP\SQLEXPRESS;Initial Catalog = master; Integrated Security = True");
        SqlCommand cmd = new SqlCommand();
        SqlDataReader dr;
        SqlDataAdapter da;
        DataSet ds = new DataSet();
        DataTable dt = new DataTable();


        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            cmd.Connection = con;
            loadlist();

        }
        public void loadlist()
        {
            con.Open();
            cmd.CommandText = "select * from tbl_VehicleType";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    comboBox1.Items.Add(dr[1].ToString());


                }
            }
            con.Close();
            con.Open();
            cmd.CommandText = "select * from tbl_ServiceDetails";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    comboBox2.Items.Add(dr[1].ToString());
                }
            }
            con.Close();

        }


        private void button1_Click(object sender, EventArgs e)
        {
            con.Open();
            cmd.CommandText = "insert into tbl_ServiceDetails values('" + textBox1.Text + "','" +
           textBox2.Text + "','" + textBox3.Text + "','" + comboBox1.SelectedItem.ToString() + "')";
            cmd.ExecuteNonQuery();
            cmd.Clone();
            MessageBox.Show("Record inserted");
            con.Close();
            textBox1.Text = "";
            textBox2.Text = "";
            textBox3.Text = "";

        }
        String a;
        private void button2_Click(object sender, EventArgs e)
        {
            con.Open();
            cmd.CommandText = "select * from tbl_VehicleType where VehicleType='" +
           textBox4.Text + "'";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    a = dr[2].ToString();
                }
            }
            con.Close();
            textBox6.Text = Convert.ToString(Convert.ToInt64(textBox5.Text) +
           Convert.ToInt64(a));

        }

        private void comboBox2_SelectedIndexChanged(object sender, EventArgs e)
        {
            con.Open();
            cmd.CommandText = "select * from tbl_ServiceDetails where VehicleNumber='" +
           comboBox2.SelectedItem.ToString() + "'";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    textBox4.Text = dr[3].ToString();
                }
            }
            con.Close();
        }
    }
}








C# Code:
using System.Data;
using System.Data.SqlClient;
using System.Collections.Generic;
using System.ComponentModel;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using static System.Windows.Forms.VisualStyles.VisualStyleElement;
using System.Diagnostics;

namespace WinFormsApp2
{
    public partial class Form1 : Form
    {
        SqlConnection con = new SqlConnection(@"Data Source=NITHIN-LAP\SQLEXPRESS;Initial Catalog = Student; Integrated Security = True");
        SqlCommand cmd = new SqlCommand();
        SqlDataReader dr;
        SqlDataAdapter da;
        DataSet ds = new DataSet();
        DataTable dt = new DataTable();
        public Form1()
        {
            InitializeComponent();
        }

        public void loadlist()
        {
            con.Open();
            cmd.CommandText = "select * from tbl_Designations";
            dr = cmd.ExecuteReader();
            if (dr.HasRows)
            {
                while (dr.Read())
                {
                    comboBox1.Items.Add(dr[0].ToString());
 comboBox2.Items.Add(dr[1].ToString());
                    
                }
            }
            con.Close();
        }
        private void button1_Click(object sender, EventArgs e)
        {
            
            con.Open();

            cmd.Connection = con;

            cmd.CommandText = "insert into tbl_EmployeeDetails(IdEmployee, EmployeeName,ContactNumber,IdDesignation,IdReportingTo) values('" +
textBox1.Text + "','" + textBox2.Text + "','" + textBox3.Text + "','" +
comboBox1.Text + "','" + textBox4.Text + "')";
            cmd.ExecuteNonQuery();
            MessageBox.Show("Record inserted");
            con.Close();
            textBox1.Text = "";
            textBox2.Text = "";
            textBox3.Text = "";
            textBox4.Text = "";
        }
        
       private void comboBox2_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (comboBox2.SelectedIndex == 0)
            {
                dt.Reset();
                con.Open();
                ds.Reset();
                ds.Tables.Add(dt);
                da = new SqlDataAdapter("select * from tbl_EmployeeDetails ", con);
                da.Fill(dt);
                dataGridView1.DataSource = dt.DefaultView;
                con.Close();
            }
            else
            {
                con.Open();
                ds.Reset();
                ds.Tables.Add(dt);
                da = new SqlDataAdapter("select * from tbl_EmployeeDetails where IdDesignation = '" + comboBox2.SelectedIndex + "'", con);

                da.Fill(dt);
                dataGridView1.DataSource = dt.DefaultView;
                con.Close();
            }
        }

        private void Form1_Load_1(object sender, EventArgs e)
        {
            cmd.Connection = con;
            loadlist();
        }
    }
}



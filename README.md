# using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient;

namespace Equipment_Management
{
    public partial class frmDeleteDepartment : Form
    {
        public frmDeleteDepartment()
        {
            InitializeComponent();
            btnDelete.Click += new EventHandler(btnDelete_Click);
        }

        void btnDelete_Click(object sender, EventArgs e)
        {
            if (lstDept.SelectedRows.Count == 1)
            {
                var row = lstDept.SelectedRows[0];
                var cell = row.Cells["id"];
                string id = (string)cell.Value;
                EquipmentEntities1 db = new EquipmentEntities1();
                Department department = db.Departments.Single(st => st.id == id);
                db.Departments.Remove(department);
                db.SaveChanges();
                this.Close();
                MessageBox.Show("Delete successfully!");
            }
            else
            {
                MessageBox.Show("You should select a department!");
            }
        }
        private void frmDeleteDepartment_Load(object sender, EventArgs e)
        {
            // Load Department
            EquipmentEntities1 db = new EquipmentEntities1();
            this.lstDept.DataSource = db.Departments.ToList(); // load all classes data and assign to combobox
            //this.cboDepartment.ValueMember = "id";
            //this.cboDepartment.DisplayMember = "Name";
            lstDept.Columns["id"].Visible = true;
            lstDept.Columns["Name"].Visible = true;
            lstDept.Columns["Rooms"].Visible = false;
        }
    }
}

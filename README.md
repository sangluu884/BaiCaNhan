using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Equipment_Management
{
    public partial class frmAddNewDepartment : Form
    {
        public frmAddNewDepartment()
        {
            InitializeComponent();
            this.Load += new EventHandler(frmAddNewDepartment_Load);
            this.btnSave.Click += new EventHandler(btnSave_Click);
        }

        private void frmAddNewDepartment_Load(object sender, EventArgs e)
        {

        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            try
            {
                string id = this.txtId.Text;
                string name = this.txtName.Text;
                EquipmentEntities1 db = new EquipmentEntities1();
                Department department = new Department();
                department.id = id;
                department.Name = name;
                db.Departments.Add(department);
                db.SaveChanges();
                this.Close();
                MessageBox.Show("Add successfully");
            }
            catch
            {

            }
        }
    }
}

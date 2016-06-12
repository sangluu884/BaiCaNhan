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
    public partial class frmAddNewRoom : Form
    {
        public frmAddNewRoom()
        {
            InitializeComponent();
            this.Load += new EventHandler(frmAddNewRoom_Load);
            this.btnSave.Click += new EventHandler(btnSave_Click);
        }

        void btnSave_Click(object sender, EventArgs e)
        {
            try
            {
                string id_Department = (string)this.cboDept.SelectedValue;
                string id = this.txtId.Text;
                string name = this.txtName.Text;
                EquipmentEntities1 db = new EquipmentEntities1();
                Room room = new Room();
                room.id = id;
                room.Name = name;
                room.id_Department = id_Department;
                db.Rooms.Add(room);
                db.SaveChanges();
                this.Close();
                MessageBox.Show("Add succefully!");
            }
            catch
            {

            }
        }

        private void frmAddNewRoom_Load(object sender, EventArgs e)
        {
            EquipmentEntities1 db = new EquipmentEntities1();
            this.cboDept.DataSource = db.Departments.ToList();
            this.cboDept.ValueMember = "id";
            this.cboDept.DisplayMember = "Name";
        }
        
    }
}

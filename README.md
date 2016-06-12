# using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Configuration;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Equipment_Management
{
    public partial class frmEquipment : Form
    {
        public frmEquipment()
        {
            InitializeComponent();
            // Register form loading event
            this.Load += new EventHandler(frmEquipment_Load);

            cboDept_Load();
        }
        public void frmEquipment_Load(object sender, EventArgs e)
        {
            // When the form is loaded
            //this.LoadEquipmentList();
            //this.LoadDepartmentList();
            //this.LoadRoom_List();
            this.btnAddNewEquipment.Click += new EventHandler(btnAddNewEquipment_Click);
            this.btnDeleteEquipment.Click += new EventHandler(btnDeleteEquipment_Click);
            this.btnAddNewCatalogue.Click += new EventHandler(btnAddNewCatalogue_Click);
            this.btnAddNewDept.Click += new EventHandler(btnAddNewDept_Click);
            this.btnAddNewRoom.Click += new EventHandler(btnAddNewRoom_Click);
            this.btnDeleteDepartment.Click += new EventHandler(btnDeleteDepartment_Click);
            this.btnDeleteRoom.Click += new EventHandler(btnDeleteRoom_Click);

            // Load Department
            //cboDept_Load();
        }
        // cboDepartment dropdown action
        private void cboDepartment_DropDown(object sender, EventArgs e)
        {
            //cboDept_Load();
        }

        // button add new room 
        void btnAddNewRoom_Click(object sender, EventArgs e)
        {
            try
            {
                frmAddNewRoom add = new frmAddNewRoom();
                add.ShowDialog();
                LoadDepartmentList();
                string room = this.cboDepartment.SelectedValue.ToString();
                cboRoom_Load(room);
            }
            catch
            {

            }
        }

        private void LoadRoom_List()
        {
            EquipmentEntities1 db = new EquipmentEntities1();
            this.cboRoom.DataSource = db.Rooms.ToList(); // load all classes data and assign to combobox
            this.cboRoom.ValueMember = "id";
            this.cboRoom.DisplayMember = "Name"; // set the display member
        }

        //button delete room
        void btnDeleteRoom_Click(object sender, EventArgs e)
        {
            try
            {
                frmDeleteRoom delete = new frmDeleteRoom();
                delete.ShowDialog();
                string room = this.cboDepartment.SelectedValue.ToString();
                cboRoom_Load(room);
            }
            catch
            {

            }
        }

        // button delete department
        void btnDeleteDepartment_Click(object sender, EventArgs e)
        {
            frmDeleteDepartment delete = new frmDeleteDepartment();
            delete.ShowDialog();
        }

        // button add new department
        void btnAddNewDept_Click(object sender, EventArgs e)
        {
            frmAddNewDepartment add = new frmAddNewDepartment();
            add.ShowDialog();
            this.LoadDepartmentList();
        }

        private void LoadDepartmentList()
        {
            // Load Department
            EquipmentEntities1 db = new EquipmentEntities1();
            this.cboDepartment.DataSource = db.Departments.ToList(); // load all classes data and assign to combobox
            this.cboDepartment.ValueMember = "id";
            this.cboDepartment.DisplayMember = "Name"; // set the display member
        }

        // button add new catalogue
        void btnAddNewCatalogue_Click(object sender, EventArgs e)
        {
            frmAddNewCatalogue add1 = new frmAddNewCatalogue();
            add1.ShowDialog();
            string catalogue = this.cboRoom.SelectedValue.ToString();
            lstCatalogue_Load(catalogue);
        }

        //SqlConnection conn = new SqlConnection(ConfigurationManager.ConnectionStrings["EquipmentEntities1"].ConnectionString);
        // SqlConnection conn = new SqlConnection("Data Source=.\\SQLEXPRESS;Initial Catalog=Equipment;Integrated Security=SSPI");
        SqlConnection conn = new SqlConnection();
        string sql;
        SqlCommand cmd;
        SqlDataAdapter sa;
        DataTable dt;

        EquipmentEntities1 db = new EquipmentEntities1();

        // load department from dtb
        private void cboDept_Load()
        {
            //EquipmentEntities1 db = new EquipmentEntities1();
            var departments = db.Departments.ToList();
            //departments.Insert(0, new Department
            //{
            //    id = "",
            //    Name = ""
            //});

            this.cboDepartment.DataSource = departments; // load all classes data and assign to combobox
            this.cboDepartment.ValueMember = "id";
            this.cboDepartment.DisplayMember = "Name"; // set the display member

            this.cboDepartment.SelectedIndex = -1;
            this.cboDepartment.Text = "Chon don vi";
        }

        // load room from dtb
        private void cboRoom_Load(string department)
        {
            try
            {

                var result = db.SP_SEARCH(department);
                //conn.Open();
                //sql = "SP_SEARCH";
                //cmd = new SqlCommand(sql, conn);
                //cmd.CommandType = CommandType.StoredProcedure;
                //cmd.Parameters.Add("@MADONVI", SqlDbType.NVarChar, 50);
                //cmd.Parameters["@MADONVI"].Value = department;
                //sa = new SqlDataAdapter(cmd);
                //dt = new DataTable();
                //sa.Fill(dt);
                cboRoom.DataSource = result;
                cboRoom.DisplayMember = "Name";
                cboRoom.ValueMember = "id";
            }
            //finally
            //{
            //    if (conn != null)
            //    {
            //        conn.Close();
            //    }
            //}
            catch
            {

            }
        }

        // load catalogue from dtb
        private void lstCatalogue_Load(string room)
        {
            try
            {
                var result = db.SP_LOADCATALOGUE(room);
                //conn.Open();
                //sql = "SP_LOADCATALOGUE";
                //cmd = new SqlCommand(sql, conn);
                //cmd.CommandType = CommandType.StoredProcedure;
                //cmd.Parameters.Add("@MAPHONG", SqlDbType.NVarChar, 50);
                //cmd.Parameters["@MAPHONG"].Value = room;
                //sa = new SqlDataAdapter(cmd);
                //dt = new DataTable();
                //sa.Fill(dt);
                lstCatalogue.DataSource = result;
                lstCatalogue.ValueMember = "id";
                lstCatalogue.DisplayMember = "Name";
            }
            //finally
            //{
            //    if (conn != null)
            //    {
            //        conn.Close();
            //    }
            //}
            catch
            {

            }
        }

        // load equipment code from database
        private void lstEquipmentCode_Load(string catalogue)
        {
            try
            {
                var result = db.SP_LOADEQUIPMENTCODE(catalogue);
                //conn.Open();
                //sql = "SP_LOADEQUIPMENTCODE";
                //cmd = new SqlCommand(sql, conn);
                //cmd.CommandType = CommandType.StoredProcedure;
                //cmd.Parameters.Add("@MADANHMUC", SqlDbType.NVarChar, 50);
                //cmd.Parameters["@MADANHMUC"].Value = catalogue;
                //sa = new SqlDataAdapter(cmd);
                //dt = new DataTable();
                //sa.Fill(dt);
                lstEquipmentCode.DataSource = result;
                lstEquipmentCode.ValueMember = "id";
                lstEquipmentCode.DisplayMember = "id";
            }
            //finally
            //{
            //    if (conn != null)
            //    {
            //        conn.Close();
            //    }
            //}
            catch
            {

            }
        }

        // load equipment from dtb to datagridview
        private void lstEquipment_Load(string id)
        {
            try
            {
                var result = db.SP_LOADEQUIPMENT(id);
                //conn.Open();
                //sql = "SP_LOADEQUIPMENT";
                //cmd = new SqlCommand(sql, conn);
                //cmd.CommandType = CommandType.StoredProcedure;
                //cmd.Parameters.Add("@MATB", SqlDbType.NVarChar, 50);
                //cmd.Parameters["@MATB"].Value = id;
                //sa = new SqlDataAdapter(cmd);
                //dt = new DataTable();
                //sa.Fill(dt);
                lstEquipment.DataSource = result;
                lstEquipment.Columns["id"].Visible = true;
                lstEquipment.Columns["Name"].Visible = true;
                lstEquipment.Columns["id_Catalogue"].Visible = false;
                lstEquipment.Columns["Price"].Visible = true;
                lstEquipment.Columns["Time_StockIn"].Visible = true;
                lstEquipment.Columns["Depreciation"].Visible = true;
                lstEquipment.Columns["Catalogue"].Visible = false;
            }
            //finally
            //{
            //    if (conn != null)
            //    {
            //        conn.Close();
            //    }
            //}
            catch
            {

            }
        }

        private void LoadEquipmentList()
        {
            try
            {
                string id = this.lstEquipmentCode.SelectedValue.ToString();
                lstEquipment_Load(id);
            }
            catch
            {

            }
        }

        // button delete equipment
        void btnDeleteEquipment_Click(object sender, EventArgs e)
        {
            if (lstEquipment.SelectedRows.Count == 1)
            {
                var row = lstEquipment.SelectedRows[0];
                var cell = row.Cells["id"];
                string id = (string)cell.Value;
                EquipmentEntities1 db = new EquipmentEntities1();
                Equipment equipment = db.Equipments.Single(st => st.id == id);
                db.Equipments.Remove(equipment);
                db.SaveChanges();
                MessageBox.Show("Delete successfully!");
                this.lstEquipmentCode.Items.Remove(id);
            }
            else
            {
                MessageBox.Show("You should select the equipment!");
            }
            this.LoadEquipmentList();
            string equip = this.lstCatalogue.SelectedValue.ToString();
            lstEquipmentCode_Load(equip);
        }

        // button add new equipment
        void btnAddNewEquipment_Click(object sender, EventArgs e)
        {
            try
            {
                frmAddNewEquipment add = new frmAddNewEquipment();
                add.ShowDialog();
                string equipment = this.lstCatalogue.SelectedValue.ToString();
                lstEquipmentCode_Load(equipment);
            }
            catch
            {

            }
        }

        // cboDepartment action
        private void cboDepartment_SelectionChangeCommitted(object sender, EventArgs e)
        {
            try
            {
                string department = (string)this.cboDepartment.SelectedValue;
                
                cboRoom_Load(department);
                string catalogue = this.cboRoom.SelectedValue.ToString();
                lstCatalogue_Load(catalogue);
                string equipment = this.lstCatalogue.SelectedValue.ToString();
                lstEquipmentCode_Load(equipment);
            }
            catch
            {
                //cboRoom.Items.Clear();
            }

        }

        // cboRoom action
        private void cboRoom_SelectionChangeCommitted(object sender, EventArgs e)
        {
            try
            {
                string catalogue = this.cboRoom.SelectedValue.ToString();
                lstCatalogue_Load(catalogue);
                string equipment = this.lstCatalogue.SelectedValue.ToString();
                lstEquipmentCode_Load(equipment);
            }
            catch
            {

            }
        }

        // lstCatalogue action
        private void lstCatalogue_MouseClick(object sender, MouseEventArgs e)
        {
            try
            {
                string equipment = this.lstCatalogue.SelectedValue.ToString();
                lstEquipmentCode_Load(equipment);
            }
            catch
            {

            }
        }

        // lstEquipmentCode action
        private void lstEquipmentCode_MouseClick(object sender, MouseEventArgs e)
        {
            try
            {
                string id = this.lstEquipmentCode.SelectedValue.ToString();
                lstEquipment_Load(id);
            }
            catch
            {

            }
        }

        // lstEquipment action
        private void lstEquipment_DoubleClick(object sender, EventArgs e)
        {
            try
            {
                if (lstEquipment.SelectedRows.Count == 1)
                {
                    var row = lstEquipment.SelectedRows[0];
                    var cell = row.Cells["id"];
                    string id = (string)cell.Value;
                    EquipmentEntities1 db = new EquipmentEntities1();
                    frmEdit edit = new frmEdit(id);
                    edit.ShowDialog();
                    this.LoadEquipmentList();
                }
            }
            catch
            {

            }
        }

        // bntDeleteCatalogue action
        private void btnDeleteCatalogue_Click(object sender, EventArgs e)
        {
            try
            {
                frmDeleteCatalogue delete = new frmDeleteCatalogue();
                delete.ShowDialog();
                string catalogue = this.cboRoom.SelectedValue.ToString();
                lstCatalogue_Load(catalogue);
            }
            catch
            {

            }
        }

        private void cboDepartment_SelectedIndexChanged(object sender, EventArgs e)
        {

        }
    }
}

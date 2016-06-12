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
using System.Data.Entity;

namespace Equipment_Management
{
    public partial class frmEdit : Form
    {
        public frmEdit(string id)
        {
            InitializeComponent();
            this.Load += new EventHandler(frmEdit_Load);
            this.btnSaveEquipment.Click += new EventHandler(btnSaveEquipment_Click);
            db = new EquipmentEntities1();
            equipment = db.Equipments.Single(st => st.id == id);
        }
        EquipmentEntities1 db;
        private Equipment equipment;
        private Department department;
        //SqlConnection conn = new SqlConnection("Data Source =.\\SQLEXPRESS;Initial Catalog=Equipment;Integrated Security=SSPI");
        //string sql;
        //SqlCommand cmd;
        //SqlDataAdapter sa;
        //DataTable dt;
        void btnSaveEquipment_Click(object sender, EventArgs e)
        {
            string id_Catalogue = (string)this.cboCatalogue.SelectedValue; // get selected id value from combobox
            string id = this.txtIdEquip.Text;
            string name = this.txtNameEquip.Text; // get inputted name from textbox
            string price = this.txtPriceEquip.Text;
            float pr = float.Parse(price);
            string time = this.txtTimeStockIn.Text;
            DateTime tm = DateTime.Parse(time);
            string depreciation = this.txtDepreciation.Text;
            equipment.id_Catalogue = id_Catalogue; // set new values to student object
            equipment.Name = name;
            equipment.id = id;
            equipment.Price = pr;
            equipment.Time_StockIn = tm;
            equipment.Depreciation = depreciation;
            db.Entry(equipment).State = EntityState.Modified; // mark student object to be modified which will be saved to database
            db.SaveChanges(); // commit the command
            this.Close(); // close the window and show message
            MessageBox.Show("Edit successfully!");
            

        }

        private void frmEdit_Load(object sender, EventArgs e)
        {
            this.cboCatalogue.DataSource = db.Catalogues.ToList();
            this.cboCatalogue.ValueMember = "id";
            this.cboCatalogue.DisplayMember = "Name";
            this.cboCatalogue.SelectedValue = equipment.id_Catalogue;
            this.txtIdEquip.Text = equipment.id;
            this.txtNameEquip.Text = equipment.Name;
            this.txtPriceEquip.Text = equipment.Price.ToString();
            this.txtTimeStockIn.Text = equipment.Time_StockIn.ToString();
            this.txtDepreciation.Text = equipment.Depreciation;

            //string dept = this.cboDept.SelectedValue.ToString();
            //cboRoom_Load(dept);

            //string catalogue = this.cboRoom.SelectedValue.ToString();
            //cboCatalogue_Load(catalogue);
        }
        //private void cboRoom_Load(string department)
        //{
        //    try
        //    {
        //        conn.Open();
        //        sql = "SP_SEARCH";
        //        cmd = new SqlCommand(sql, conn);
        //        cmd.CommandType = CommandType.StoredProcedure;
        //        cmd.Parameters.Add("@MADONVI", SqlDbType.NVarChar, 50);
        //        cmd.Parameters["@MADONVI"].Value = department;
        //        sa = new SqlDataAdapter(cmd);
        //        dt = new DataTable();
        //        sa.Fill(dt);
        //        cboRoom.DataSource = dt;
        //        cboRoom.DisplayMember = "Name";
        //        cboRoom.ValueMember = "id";
        //    }
        //    finally
        //    {
        //        if (conn != null)
        //        {
        //            conn.Close();
        //        }
        //    }
        //}

        //private void cboCatalogue_Load(string room)
        //{
        //    try
        //    {
        //        conn.Open();
        //        sql = "SP_LOADCATALOGUE";
        //        cmd = new SqlCommand(sql, conn);
        //        cmd.CommandType = CommandType.StoredProcedure;
        //        cmd.Parameters.Add("@MAPHONG", SqlDbType.NVarChar, 50);
        //        cmd.Parameters["@MAPHONG"].Value = room;
        //        sa = new SqlDataAdapter(cmd);
        //        dt = new DataTable();
        //        sa.Fill(dt);
        //        cboId_Catalogue.DataSource = dt;
        //        cboId_Catalogue.ValueMember = "id";
        //        cboId_Catalogue.DisplayMember = "Name";
        //    }
        //    finally
        //    {
        //        if (conn != null)
        //        {
        //            conn.Close();
        //        }
        //    }
        //}

        //private void cboDept_SelectionChangeCommitted(object sender, EventArgs e)
        //{
        //    string dept = this.cboDept.SelectedValue.ToString();
        //    cboRoom_Load(dept);
        //}

        //private void cboRoom_SelectionChangeCommitted(object sender, EventArgs e)
        //{
        //    string catalogue = this.cboRoom.SelectedValue.ToString();
        //    cboCatalogue_Load(catalogue);
        //}
    }
}

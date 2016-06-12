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
    public partial class frmDeleteCatalogue : Form
    {
        public frmDeleteCatalogue()
        {
            InitializeComponent();
            this.Load += new EventHandler(frmDeleteCatalogue_Load);
            //this.btnDelete.Click += new EventHandler(btnDelete_Click);
            this.btnDelete.Click += new EventHandler(btnDelete_Click);
        }

        void btnDelete_Click(object sender, EventArgs e)
        {
            try
            {
                if (lstCatalogue.SelectedRows.Count == 1)
                {
                    var row = lstCatalogue.SelectedRows[0];
                    var cell = row.Cells["id"];
                    string id = (string)cell.Value;
                    EquipmentEntities1 db = new EquipmentEntities1();
                    Catalogue catalogue = db.Catalogues.Single(st => st.id == id);
                    db.Catalogues.Remove(catalogue);
                    db.SaveChanges();
                    this.Close();
                    MessageBox.Show("Delete successfully!");
                }
                else
                {
                    MessageBox.Show("You should select a catalogue!");
                }
            }
            catch
            {

            }
        }

        private void frmDeleteCatalogue_Load(object sender, EventArgs e)
        {
            this.cboDepartment.DropDown += new EventHandler(cboDepartment_DropDown);
            // select room
            //string department = (string)this.cboDepartment.SelectedValue;
            //cboRoom_Load(department);
        }

        //SqlConnection conn = new SqlConnection("Data Source =.\\SQLEXPRESS;Initial Catalog=Equipment;Integrated Security=SSPI");
        SqlConnection conn = new SqlConnection();
        string sql;
        SqlCommand cmd;
        SqlDataAdapter sa;
        DataTable dt;

        EquipmentEntities1 db = new EquipmentEntities1();

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

        private void DeleteCatalogue_Load(string catalogue)
        {
            try
            {
                var result = db.SP_LOADCATALOGUE(catalogue);
                //conn.Open();
                //sql = "SP_LOADCATALOGUE";
                //cmd = new SqlCommand(sql, conn);
                //cmd.CommandType = CommandType.StoredProcedure;
                //cmd.Parameters.Add("@MAPHONG", SqlDbType.NVarChar, 50);
                //cmd.Parameters["@MAPHONG"].Value = catalogue;
                //sa = new SqlDataAdapter(cmd);
                //dt = new DataTable();
                //sa.Fill(dt);
                lstCatalogue.DataSource = result;
                //lstCatalogue.ValueMember = "id";
                //lstCatalogue.DisplayMember = "Name";
                lstCatalogue.Columns["id"].Visible = true;
                lstCatalogue.Columns["Name"].Visible = true;
                lstCatalogue.Columns["id_Room"].Visible = false;
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

        private void cboDepartment_SelectionChangeCommitted(object sender, EventArgs e)
        {
            try
            {
                string department = (string)this.cboDepartment.SelectedValue;
                cboRoom_Load(department);
                string catalogue = (string)this.cboRoom.SelectedValue;
                DeleteCatalogue_Load(catalogue);
            }
            catch
            {

            }
        }

        private void cboRoom_SelectionChangeCommitted(object sender, EventArgs e)
        {
            try
            {
                string catalogue = (string)this.cboRoom.SelectedValue;
                DeleteCatalogue_Load(catalogue);
            }
            catch
            {

            }
        }

        private void cboDepartment_DropDown(object sender, EventArgs e)
        {
                // Load Department
                //EquipmentEntities1 db = new EquipmentEntities1();
                this.cboDepartment.DataSource = db.Departments.ToList(); // load all classes data and assign to combobox
                this.cboDepartment.ValueMember = "id";
                this.cboDepartment.DisplayMember = "Name"; // set the display member
        }


    }
}

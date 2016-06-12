using System;
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
    public partial class frmAddNewEquipment : Form
    {
        public frmAddNewEquipment()
        {
            InitializeComponent();
            this.Load += new EventHandler(frmAddNewEquipment_Load);
            this.btnSaveEquipment.Click += new EventHandler(btnSaveEquipment_Click);
        }

        //SqlConnection conn = new SqlConnection("Data Source =.\\SQLEXPRESS;Initial Catalog=Equipment;Integrated Security=SSPI");
        SqlConnection conn = new SqlConnection();
        string sql;
        SqlCommand cmd;
        SqlDataAdapter sa;
        DataTable dt;

        EquipmentEntities1 db = new EquipmentEntities1();

        private void frmAddNewEquipment_Load(object sender, EventArgs e)
        {
            
            this.cboDept.DataSource = db.Departments.ToList();
            this.cboDept.ValueMember = "id";
            this.cboDept.DisplayMember = "Name";

            string dept = this.cboDept.SelectedValue.ToString();
            cboRoom_Load(dept);

            string catalogue = this.cboRoom.SelectedValue.ToString();
            cboCatalogue_Load(catalogue);

        }

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

        private void cboCatalogue_Load(string room)
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
                cboId_Catalogue.DataSource = result;
                cboId_Catalogue.ValueMember = "id";
                cboId_Catalogue.DisplayMember = "Name";
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

        void btnSaveEquipment_Click(object sender, EventArgs e)
        {
            string id_Catalogue = (string)this.cboId_Catalogue.SelectedValue.ToString();
            string id = this.txtIdEquip.Text;
            string name = this.txtNameEquip.Text;
            string price = this.txtPriceEquip.Text;
            float pr = float.Parse(price);
            string time = this.txtTimeStockIn.Text;
            DateTime tm = DateTime.Parse(time);
            string depreciation = this.txtDepreciation.Text;
            //EquipmentEntities1 db = new EquipmentEntities1();
            Equipment equipment = new Equipment();
            equipment.id = id;
            equipment.Name = name;
            equipment.id_Catalogue = id_Catalogue;
            equipment.Price = pr;
            equipment.Time_StockIn = tm;
            equipment.Depreciation = depreciation;
            db.Equipments.Add(equipment);
            db.SaveChanges();
            this.Close();
            MessageBox.Show("Add successfully!");
        }

        private void cboDept_SelectionChangeCommitted(object sender, EventArgs e)
        {
            string dept = this.cboDept.SelectedValue.ToString();
            cboRoom_Load(dept);
            string catalogue = this.cboRoom.SelectedValue.ToString();
            cboCatalogue_Load(catalogue);
        }

        private void cboRoom_SelectionChangeCommitted(object sender, EventArgs e)
        {
            string catalogue = this.cboRoom.SelectedValue.ToString();
            cboCatalogue_Load(catalogue);
        }
    }
}

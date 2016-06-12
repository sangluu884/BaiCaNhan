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
    public partial class frmDeleteRoom : Form
    {
        public frmDeleteRoom()
        {
            InitializeComponent();
            this.Load += new EventHandler(frmDeleteRoom_Load);
            this.btnDelete.Click += new EventHandler(btnDelete_Click);
        }

        EquipmentEntities1 db = new EquipmentEntities1();

        private void frmDeleteRoom_Load(object sender, EventArgs e)
        {
            // Load Department
            
            this.cboDepartment.DataSource = db.Departments.ToList(); // load all classes data and assign to combobox
            this.cboDepartment.ValueMember = "id";
            this.cboDepartment.DisplayMember = "Name"; // set the display member

            string room = (string)this.cboDepartment.SelectedValue;
            lstRoom_Load(room);
        }
        //SqlConnection conn = new SqlConnection("Data Source =.\\SQLEXPRESS;Initial Catalog=Equipment;Integrated Security=SSPI");
        SqlConnection conn = new SqlConnection();
        string sql;
        SqlCommand cmd;
        SqlDataAdapter sa;
        DataTable dt;

        private void lstRoom_Load(string department)
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
                lstRoom.DataSource = result;
                lstRoom.Columns["id"].Visible = true;
                lstRoom.Columns["Name"].Visible = true;
                //lstRoom.Columns["id_Department"].Visible = false;
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
                string room = (string)this.cboDepartment.SelectedValue;
                lstRoom_Load(room);
            }
            catch
            {

            }
        }

        private void btnDelete_Click(object sender, EventArgs e)
        {
            try
            {
                if (lstRoom.SelectedRows.Count == 1)
                {
                    var row = lstRoom.SelectedRows[0];
                    var cell = row.Cells["id"];
                    string id = (string)cell.Value;
                    //EquipmentEntities1 db = new EquipmentEntities1();
                    Room room = db.Rooms.Single(st => st.id == id);
                    db.Rooms.Remove(room);
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
    }
}

# Code-with-Sql-vp

using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace YourNamespace
{
    public partial class frmCustomerDataEntry : Form
    {
        public frmCustomerDataEntry()
        {
            InitializeComponent();
        }

        private void frmCustomerDataEntry_Load(object sender, EventArgs e)
        {
            loadCustomer();
        }

        private void loadCustomer()
        {
            string strConnection = "Data Source=.\\sqlexpress;Initial Catalog=CustomerDB;Integrated Security=True";

            try
            {
                using (SqlConnection objConnection = new SqlConnection(strConnection))
                {
                    objConnection.Open();
                    string strCommand = "SELECT * FROM CustomerTable";
                    using (SqlCommand objCommand = new SqlCommand(strCommand, objConnection))
                    {
                        DataSet objDataSet = new DataSet();
                        SqlDataAdapter objAdapter = new SqlDataAdapter(objCommand);
                        objAdapter.Fill(objDataSet);

                        if (dtgCustomer != null)
                        {
                            dtgCustomer.DataSource = objDataSet.Tables[0];
                        }
                        else
                        {
                            MessageBox.Show("DataGridView is not initialized.");
                        }
                    }
                }
            }
            catch (SqlException sqlEx)
            {
                MessageBox.Show($"SQL Error: {sqlEx.Message}");
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error: {ex.Message}");
            }
        }
    }
}

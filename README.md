using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Data;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace KTDDotNetCore.ConsoleApp
{
    internal class AdoDotNetExample
    {
        private readonly SqlConnectionStringBuilder _sqlConnectionStringBuilder = new SqlConnectionStringBuilder()
        {
            DataSource = "AGD-PC-535",
            InitialCatalog = "DotNetTraningBath4",
            UserID = "sa",
            Password = "sa@123"
        };

      public void Read()
        {
            //SqlConnectionStringBuilder stringBuilder = new SqlConnectionStringBuilder();
           // stringBuilder.DataSource = "AGD-PC-535";
           // stringBuilder.InitialCatalog = "DotNetTraningBath4";
          //  stringBuilder.UserID = "sa";
          //  stringBuilder.Password = "sa@123";

            SqlConnection connection = new SqlConnection(_sqlConnectionStringBuilder.ConnectionString);

            //SqlConnection connection = new SqlConnection("Data Source = AGD - PC - 535; Initial Catalog = DotNetTraningBath4; User ID = sa; Password = sa@123");
            connection.Open();

            string query = "select * from Tbl_Blg";
            SqlCommand cmd = new SqlCommand(query, connection);
            SqlDataAdapter SqlDataAdapter = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            SqlDataAdapter.Fill(dt);

            connection.Close();

            foreach (DataRow dr in dt.Rows)
            {
                Console.WriteLine("Blog ID" + dr["BlogID"]);
                Console.WriteLine("Blog Title" + dr["BlogTitle"]);
                Console.WriteLine("Blog Author" + dr["BlogAuthor"]);
                Console.WriteLine("Blog Content" + dr["BlogContent"]);
                Console.WriteLine("----------------------");
            }
        }
        public void Create(String Title,String Author,String Content)
        {
            SqlConnection connection = new SqlConnection(_sqlConnectionStringBuilder.ConnectionString);
            connection.Open();

            String query = @"INSERT INTO [dbo].[Tbl_Blg]
                            (   [BlogTitle]
                                ,[BlogAuthor]
                                ,[BlogContent])
                            VALUES
                                 (@BlogTitle,
                                  @BlogAuthor,
                                  @BlogContent)";

            SqlCommand cmd = new SqlCommand(query,connection);
            cmd.Parameters.AddWithValue("@BlogTitle", Title);
            cmd.Parameters.AddWithValue("@BlogAuthor", Author);
            cmd.Parameters.AddWithValue("@BlogContent", Content);

            int result = cmd.ExecuteNonQuery();

            connection.Close();
            String message = result > 0 ? "Saving Successful." : "Saving Successful Failed.";
            Console.WriteLine(message);
        }
    }
}

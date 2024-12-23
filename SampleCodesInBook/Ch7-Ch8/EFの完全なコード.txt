using Microsoft.EntityFrameworkCore;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace BlazorApp1.Data
{
    public partial class PubsDbContext : DbContext
    {
        public PubsDbContext(DbContextOptions<PubsDbContext> options) : base(options)
        {
        }

        public DbSet<Author> Authors { get; set; } = null!;
        public DbSet<Title> Titles { get; set; } = null!;
        public DbSet<Publisher> Publishers { get; set; } = null!;
        public DbSet<Store> Stores { get; set; } = null!;
        public DbSet<Sale> Sales { get; set; } = null!;
        public DbSet<TitleAuthor> TitleAuthors { get; set; } = null!;

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // 複合 PK の指定 (Fluent API でしか定義できない)
            modelBuilder.Entity<Sale>().HasKey(s => new { s.StoreId, s.OrderNumber, s.TitleId });
            modelBuilder.Entity<TitleAuthor>().HasKey(ta => new { ta.AuthorId, ta.TitleId });
        }
    }

    [Table("authors")]
    public partial class Author
    {
        [Column("au_id"), Required, MaxLength(11), Key]
        public string AuthorId { get; set; } = null!;

        [Column("au_fname"), Required, MaxLength(20)]
        public string AuthorFirstName { get; set; } = null!;

        [Column("au_lname"), Required, MaxLength(40)]
        public string AuthorLastName { get; set; } = null!;

        [Column("phone"), Required, MaxLength(12)]
        public string Phone { get; set; } = null!;

        [Column("address"), MaxLength(40)]
        public string? Address { get; set; }

        [Column("city"), MaxLength(20)]
        public string? City { get; set; }

        [Column("state"), MaxLength(2)]
        public string? State { get; set; }

        [Column("zip"), MaxLength(5)]
        public string? Zip { get; set; }

        [Column("contract"), Required]
        public bool Contract { get; set; }

        [Column("rowversion"), Timestamp, ConcurrencyCheck]
        public byte[]? RowVersion { get; set; }

        public ICollection<TitleAuthor> TitleAuthors { get; set; } = new HashSet<TitleAuthor>();

    }

    [Table("publishers")]
    public partial class Publisher
    {
        [Column("pub_id"), Required, MaxLength(4)]
        public string PublisherId { get; set; } = null!;

        [Column("pub_name"), MaxLength(40)]
        public string? PublisherName { get; set; }

        [Column("city"), MaxLength(20)]
        public string? City { get; set; }

        [Column("state"), MaxLength(2)]
        public string? State { get; set; }

        [Column("country"), MaxLength(30)]
        public string? Country { get; set; }

        public ICollection<Title> Titles { get; set; } = new HashSet<Title>();
    }

    [Table("titles")]
    public partial class Title
    {
        [Column("title_id"), Required, MaxLength(6), Key]
        public string TitleId { get; set; } = null!;

        [Column("title"), Required, MaxLength(80)]
        public string TitleName { get; set; } = null!;

        [Column("type"), Required, MaxLength(12)]
        public string Type { get; set; } = null!;

        [Column("price")]
        public decimal? Price { get; set; }

        [Column("advance")]
        public decimal? Advance { get; set; }

        [Column("royalty")]
        public int? Royalty { get; set; }

        [Column("ytd_sales")]
        public int? YeatToDateSales { get; set; }

        [Column("notes"), MaxLength(200)]
        public string? Notes { get; set; }

        [Column("pubdate"), Required]
        public DateTime PublishedDate { get; set; }

        [Column("pub_id"), MaxLength(4)]
        public string? PublisherId { get; set; }

        public Publisher? Publisher { get; set; }

        public ICollection<Sale> Sales { get; set; } = new HashSet<Sale>();

        public ICollection<TitleAuthor> TitleAuthors { get; set; } = new HashSet<TitleAuthor>();
    }

    [Table("sales")]
    public partial class Sale
    {
        // ※ 複合キーは Data Annotation で指定できないため、Fluent API を使う
        [Column("stor_id"), Required, MaxLength(4)]
        public string StoreId { get; set; } = null!;

        [Column("ord_num"), Required, MaxLength(20)]
        public string OrderNumber { get; set; } = null!;

        [Column("ord_date"), Required]
        public DateTime OrderDate { get; set; }

        [Column("qty"), Required]
        public int Quantity { get; set; }

        [Column("payterms"), Required, MaxLength(12)]
        public string PayTerms { get; set; } = null!;

        [Column("title_id"), Required, MaxLength(6)]
        public string TitleId { get; set; } = null!;

        public Store Store { get; set; } = null!;

        public Title Title { get; set; } = null!;
    }

    [Table("stores")]
    public partial class Store
    {
        [Column("stor_id"), Required, MaxLength(4), Key]
        public string StoreId { get; set; } = null!;

        [Column("stor_name"), Required, MaxLength(40)]
        public string StoreName { get; set; } = null!;

        [Column("stor_addr"), Required, MaxLength(40)]
        public string Address { get; set; } = null!;

        [Column("city"), Required, MaxLength(20)]
        public string City { get; set; } = null!;

        [Column("state"), Required, MaxLength(22)]
        public string State { get; set; } = null!;

        [Column("zip"), Required, MaxLength(5)]
        public string Zip { get; set; } = null!;

        public ICollection<Sale> Sales { get; set; } = new HashSet<Sale>();
    }

    [Table("titleauthor")]
    public partial class TitleAuthor
    {
        [Column("au_id"), Required]
        public string AuthorId { get; set; } = null!;

        [Column("title_id"), Required]
        public string TitleId { get; set; } = null!;

        [Column("au_ord")]
        public byte? AuthorOrder { get; set; }

        [Column("royaltyper")]
        public int RoyaltyPercentage { get; set; }

        public Author Author { get; set; } = null!;

        public Title Title { get; set; } = null!;

    }
}

